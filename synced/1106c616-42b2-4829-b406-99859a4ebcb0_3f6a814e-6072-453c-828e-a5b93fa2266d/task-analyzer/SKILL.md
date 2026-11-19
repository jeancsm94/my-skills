---
name: task-analyzer
description: Lê uma tarefa/ticket do ERP (tracker próprio em devtracker.vendaerp.com.br, atrás de login) e entrega uma análise estruturada — resumo do que a task realmente pede, plano de implementação passo a passo referenciando o código, e riscos/perguntas em aberto. Use SEMPRE que o usuário pedir para analisar, entender, ler ou "destrinchar" uma task/tarefa/ticket/card/issue do ERP, colar um link de task do devtracker.vendaerp.com.br, perguntar "o que essa task está pedindo", "o que eu preciso fazer nessa tarefa" ou "por onde começo nessa task" — mesmo que não use a palavra "skill" nem "analisar" explicitamente.
---

# Task Analyzer

Transforma uma tarefa do ERP num documento de análise acionável. O objetivo não é repetir o ticket, e sim **interpretar a intenção** e devolver um plano que o desenvolvedor consiga executar.

A entrega tem sempre três blocos: **Resumo do que a task pede**, **Plano de implementação** e **Riscos e perguntas em aberto**.

## Passo 1 — Obter a task

A task fica no tracker próprio do ERP (`devtracker.vendaerp.com.br`), atrás de login, e não tem conector/API exposto. Portanto:

- O usuário aponta a task por **URL completa** (preferível) ou por **ID/número**. Prefira sempre a URL. Se vier só o ID e você ainda não conhece o padrão de URL do devtracker, peça **uma** URL de exemplo, aprenda o padrão e reutilize para os próximos IDs.
- Use o **navegador (Claude no Chrome)** com a **sessão já logada** do usuário. **Nunca peça senha nem credenciais** — aproveite a sessão existente (é por isso que o acesso a "todas as informações" depende de você estar logado no navegador). Se a página redirecionar para login (sessão expirada), peça ao usuário para logar e retome de onde parou.
- Abra a página e extraia **semanticamente** (não dependa de seletores fixos, pois o layout pode mudar):
  - Título e ID
  - Status / coluna / etapa
  - Descrição completa
  - Critérios de aceite (se houver)
  - Comentários e discussão (onde costuma estar o contexto real)
  - Anexos e prints — **leia o conteúdo das imagens**, pois muita regra de negócio aparece em screenshots
  - Itens vinculados (tasks relacionadas, dependências)
  - Autor e responsável
- Se no futuro existir um conector/MCP para o ERP, prefira o conector ao navegador.

## Passo 2 — Entender o contexto de código

Faça este passo **somente se o repositório do ERP estiver disponível localmente** (aberto no Cursor/Claude Code). Caso contrário, pule e sinalize no documento que o plano é mais geral por falta de acesso ao código.

1. Leia `references/erp-architecture.md` para saber a arquitetura, as camadas e onde procurar cada tipo de coisa.
2. Extraia da task os **termos concretos**: nomes de telas, entidades, endpoints, módulos, mensagens de erro.
3. Busque esses termos no repositório para localizar os pontos reais de mudança. Procure em todas as camadas plausíveis (backend C#, API, frontend AngularJS, mobile, persistência).
4. Identifique **quais camadas a task atravessa** — boa parte das mudanças no ERP toca mais de uma (ex: API + frontend, ou backend + mobile + migração de dados).

## Passo 3 — Escrever a análise

Use SEMPRE este template, em português:

```
# Análise — Task [ID]: [Título]

## Resumo (o que a task pede)
[2 a 4 frases, em linguagem clara, interpretando a INTENÇÃO: qual é o objetivo,
qual problema resolve e qual o resultado esperado. Não copie o texto do ticket —
explique o que ele significa na prática.]

## Plano de implementação
1. [Passo acionável] — [onde mexer no código: camada / módulo / arquivo, quando o
   repositório estiver disponível. Cite endpoints, telas e entidades concretas.]
2. ...
[Ordene os passos numa sequência executável. Se a task atravessa várias camadas,
deixe explícito a ordem — ex: modelo/migração → API → frontend → mobile.]

## Riscos e perguntas em aberto
- [Ambiguidades, informação faltando, casos extremos e decisões que precisam ser
  confirmadas com quem abriu a task. Liste como perguntas objetivas.]
```

Se o repositório **não** estiver disponível, acrescente uma linha no topo do documento:
`> ⚠️ Análise feita sem acesso ao código — o plano é de alto nível e as referências de arquivo são suposições.`

## Princípios de qualidade

- **Interprete, não repita.** Se o resumo apenas reescreve o ticket, não agrega valor. Diga o que a task realmente quer dizer.
- **Separe o explícito do inferido.** Quando você deduzir algo que não está escrito, marque como suposição (no plano ou nos riscos), nunca apresente como fato.
- **Diante de ambiguidade, pergunte — não chute.** Toda lacuna relevante vira item em "Riscos e perguntas em aberto".
- **Seja específico e acionável.** "Atualizar o backend" é fraco; "adicionar o campo X no endpoint Y e propagar para o DTO Z" é útil.

## Casos de borda

- **Sessão expirada / redireciona pro login:** peça ao usuário para logar no navegador e retome. Nunca tente contornar a autenticação.
- **Task vaga ou de uma linha só:** entregue mesmo assim, mas com o resumo curto e o peso maior em "Riscos e perguntas em aberto" — esse é o caso em que as perguntas valem mais que o plano.
- **Task multi-parte (várias coisas num ticket só):** numere cada sub-objetivo no resumo e organize o plano por sub-objetivo.
- **Prints/anexos com regra de negócio:** leia as imagens e incorpore o que elas dizem; não as ignore.
- **Task que cruza várias camadas:** deixe a ordem de execução explícita no plano (ex: o que precisa existir no backend antes do frontend conseguir consumir).
