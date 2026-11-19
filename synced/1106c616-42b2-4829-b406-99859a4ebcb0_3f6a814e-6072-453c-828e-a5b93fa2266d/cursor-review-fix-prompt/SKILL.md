---
name: cursor-review-fix-prompt
description: Converte um markdown com os apontamentos de um code review (erros, problemas, sugestões por arquivo) em prompts prontos pra colar no Cursor que corrigem cada ponto, seguindo as best practices oficiais de agentes do Cursor. Use SEMPRE que o usuário colar ou anexar um markdown/relatório de code review, lista de comentários de PR, ou apontamentos de revisão e pedir "gera os prompts pra corrigir", "transforma isso em prompt pro Cursor", "como eu mando o Cursor arrumar esses pontos" — mesmo que ele não diga a palavra "skill" nem "prompt" explicitamente.
---

# Cursor Review-Fix Prompt

Transforma os apontamentos de um code review (em markdown) em prompts sequenciados, em inglês, prontos pra colar no Cursor. O objetivo é que cada correção seja feita pelo agente de forma precisa, sem drift e sem mexer no que não foi pedido — aplicando as best practices oficiais do Cursor (`cursor.com/docs/cloud-agent/best-practices`).

## Entrada

Um markdown com os erros/sugestões do code review. Pode vir como bloco colado ou arquivo. Cada apontamento normalmente tem: arquivo (e talvez linha), descrição do problema e, às vezes, severidade ou sugestão de correção. Nem todo review é bem formatado — extraia o que der.

## Fluxo

1. **Parse**: extraia cada apontamento como `{arquivo, local, problema, correção sugerida, severidade}`. Se faltar arquivo/local, marque como `a localizar` e instrua o agente a localizar via busca.
2. **Classifique** cada apontamento:
   - **Pequeno**: localizado, baixo risco, sem mudar comportamento (ex.: usar `??` no lugar de `||`, renomear, guard de null).
   - **Grande**: multi-arquivo, muda comportamento, toca fluxo crítico (auth, integração, boleto/CNAB, persistência) ou o review não cravou a causa.
3. **Agrupe**: junte vários apontamentos pequenos do mesmo arquivo/área num único prompt (Template A). Isole cada apontamento grande no seu próprio prompt (Template B). Bug de runtime que o review não conseguiu reproduzir → use o prompt de Debug Mode.
4. **Gere os prompts** usando os templates abaixo, preenchendo os `{{PLACEHOLDERS}}`.
5. **Ordene** por dependência e risco (correções que destravam outras primeiro; mudanças arriscadas isoladas e por último). Entregue na ordem de execução.

## Princípios das best practices embutidos em todo prompt

Não cite a doc dentro do prompt — apenas aplique. O que cada prompt precisa carregar:

- **Seja específico.** Nomeie o arquivo, o problema exato e o comportamento esperado. Prompt vago ("arruma os erros") tem taxa de sucesso baixa; prompt com arquivo + caso de borda + padrão a seguir é o que funciona.
- **Dê contexto.** Mande o agente consultar `AGENTS.md` e `.cursor/rules` e seguir os padrões existentes do repo, em vez de inventar padrão novo. O agente é um dev esperto, mas de baixo contexto: ele acerta quando você dá o contexto.
- **Planeje antes de editar.** Para qualquer coisa não-trivial, o agente produz um plano curto e **espera aprovação** antes de mexer no código (mecanismo anti-drift / Plan mode). Isso evita que ele saia mudando o repo inteiro.
- **Feche o loop.** Diga quais checagens rodar (build, lint, o caso que estava falhando) para o agente verificar o próprio trabalho antes de terminar.
- **Escopo apertado.** Corrija só o que o review apontou. Nada de refactor não pedido, scaffolding de teste, nem async/await em bloco.

## Convenções do projeto a forçar nas correções

Todo prompt gerado deve exigir que o agente respeite:

- Usar `??` em vez de `||` para defaults numéricos.
- Garantir boolean com `?? false` e array com `?? []`.
- Extrair helpers pequenos quando melhora a legibilidade.
- Angular: padrão classe/model TypeScript enviado ao backend; **não** usar Reactive Forms.
- Auth: baseada em cookie, server-side; **sem** JWT e sem mudança de arquitetura, mantendo compatibilidade com o ERP legado.

Inclua só as convenções relevantes ao arquivo em questão (ex.: não cite Reactive Forms num fix de C#).

## Template A — Correções pequenas / em lote

Um prompt cobrindo N apontamentos localizados do mesmo arquivo/área.

```text
You are fixing issues flagged in code review. Apply ONLY the fixes listed below — do not refactor anything else, do not add tests, do not add blanket async/await.

Context: follow AGENTS.md and .cursor/rules. Match the existing patterns and naming in the repo. For any file you change, output the COMPLETE file in the repo's pattern. Use `??` (not `||`) for numeric/default fallbacks; guarantee booleans with `?? false` and arrays with `?? []`; extract small helpers when it improves readability.

Fixes:
1. {{FILE}} ({{LOCATION}}) — {{ISSUE}}. Expected: {{EXPECTED}}.
2. {{FILE}} ({{LOCATION}}) — {{ISSUE}}. Expected: {{EXPECTED}}.

When done, run {{CHECKS}} and report what changed in each file.
```

## Template B — Correção grande / arriscada (com plano)

Um prompt por apontamento grande. Trava em fase de plano antes de editar.

```text
You are fixing a complex issue flagged in code review.

PHASE 1 — PLAN (do NOT edit code yet):
Area: {{FILE_OR_AREA}}. Reported issue: {{ISSUE}}. Expected behavior: {{EXPECTED}}.
Investigate the relevant code, then write a short plan to PLAN.md: root cause, the exact files you'll touch, the change in each, and the risks. Then STOP and wait for my approval.

PHASE 2 — IMPLEMENT (only after I approve PLAN.md):
Apply the plan exactly and stay within the files listed in PLAN.md. Follow AGENTS.md and .cursor/rules and the existing patterns. Output the COMPLETE contents of every file you change. Use `??` over `||`; booleans `?? false`, arrays `?? []`; extract helpers where it clarifies intent. No unrelated changes, no test scaffolding, no blanket async/await. If reality diverges from PLAN.md, update PLAN.md and flag the divergence before continuing.

VERIFY: run {{CHECKS}} and the specific failing case; report the results.
```

## Prompt de Debug Mode — bug de runtime sem causa cravada

Quando o review apontou um sintoma mas não a causa.

```text
You are debugging a runtime issue flagged in code review that has no confirmed root cause.

Do NOT guess a fix yet. First add targeted instrumentation/logging to reproduce and isolate the problem. Repro steps: {{REPRO_STEPS}}. Observed: {{OBSERVED}}. Expected: {{EXPECTED}}.
Report the root cause you found and a proposed fix in PLAN.md, then wait for my approval before changing behavior. Follow AGENTS.md / .cursor/rules and the repo's patterns.
```

## Formato de saída

Entregue em markdown: uma linha de cabeçalho por prompt com `severidade · arquivo/área`, seguida do prompt no seu próprio bloco ```text```. Se houver mais de um, feche com a **ordem de execução sugerida** em uma linha. Sem explicação longa antes ou depois — o Jean prefere conciso e quer copiar e colar direto.

## Exemplo

**Input (trecho do review):**
```markdown
- src/app/lancamentos/edit-lancamentos.component.ts:142 — `valor || 0` deveria usar `??` (0 é valor válido).
- src/app/lancamentos/edit-lancamentos.component.ts:160 — `getParcelas()` pode retornar undefined; consumidor faz `.length` e quebra.
- services/BoletoService.cs — IndicadorPermissaoRecebimentoParcial está hardcoded "S"; precisa vir da config do lançamento. Toca o fluxo de geração de boleto BB.
```

**Output:**

`pequeno · edit-lancamentos.component.ts`
```text
You are fixing issues flagged in code review. Apply ONLY the fixes listed below — do not refactor anything else, do not add tests, do not add blanket async/await.

Context: follow AGENTS.md and .cursor/rules. Match the existing patterns and naming in the repo. For any file you change, output the COMPLETE file in the repo's pattern. Use `??` (not `||`) for numeric/default fallbacks; guarantee arrays with `?? []`; extract small helpers when it improves readability.

Fixes:
1. src/app/lancamentos/edit-lancamentos.component.ts (line 142) — `valor || 0` treats a valid 0 as missing. Expected: use `valor ?? 0`.
2. src/app/lancamentos/edit-lancamentos.component.ts (line 160) — `getParcelas()` may return undefined and the caller does `.length`. Expected: guarantee an array with `getParcelas() ?? []` at the call site.

When done, run the project build and lint, and report what changed in each file.
```

`grande · BoletoService.cs (fluxo de boleto BB)`
```text
You are fixing a complex issue flagged in code review.

PHASE 1 — PLAN (do NOT edit code yet):
Area: services/BoletoService.cs and the lançamento → DtoBoleto → BoletoBody chain. Reported issue: IndicadorPermissaoRecebimentoParcial is hardcoded as "S". Expected behavior: it must come from the lançamento configuration.
Investigate the relevant code, then write a short plan to PLAN.md: root cause, the exact files you'll touch, the change in each, and the risks to the Banco do Brasil boleto generation flow. Then STOP and wait for my approval.

PHASE 2 — IMPLEMENT (only after I approve PLAN.md):
Apply the plan exactly and stay within the files listed in PLAN.md. Follow AGENTS.md and .cursor/rules and the existing patterns. Output the COMPLETE contents of every file you change. Use `??` over `||`; booleans `?? false`. No unrelated changes, no test scaffolding, no blanket async/await. If reality diverges from PLAN.md, update PLAN.md and flag it.

VERIFY: build the solution and exercise the BB boleto generation path; report the results.
```

Ordem sugerida: 1) edit-lancamentos (pequeno), 2) BoletoService (grande, com plano).
