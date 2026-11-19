---
name: pressel-google-ads
description: "Cria uma página de pré-venda (pressel) transparente para produto de afiliado e a campanha completa de Google Ads, a partir da URL do produto e do país alvo."
---

# Pressel + Campanha Google Ads (afiliado)

Fluxo em três partes: (A) página de pré-venda em HTML, (B) anúncio completo para Google Ads e (C) CPA desejado e orçamento. Todas usam os mesmos dados; a página e o anúncio se conectam pelas âncoras #1 a #6.

## ENTRADAS

Obrigatórias (se faltar alguma, pergunte tudo de uma vez só):
- URL do produto ou link de afiliado
- Idioma e país alvo (se o usuário não souber o idioma, use o idioma oficial mais falado do país e diga qual foi usado, ex.: Noruega → norueguês bokmål)
- Preço, desconto, valor economizado e frete (o usuário pode responder "verificar no site": nesse caso, levante tudo na Etapa 1)
- Payout (comissão por venda, com moeda)

Opcionais (use o padrão se não vier):
- Nome do produto (padrão: o da página oficial, grafado exatamente igual)
- URL final da pressel (padrão: `https://SEU-DOMINIO.com/NOME-DO-PRODUTO`). Repita a URL para o usuário e confirme a grafia do caminho (ex.: `/katori` x `/katory`), porque ela vai em todos os sitelinks.
- Estilo da página: Publieditorial (padrão) | Review | Comparativo. Se o usuário pedir "redirecionar para a venda do produtor", entenda como CTAs que levam ao produtor, nunca redirecionamento automático, e explique isso em uma frase.
- Moeda da conta Google Ads (padrão: BRL)
- Taxa de aprovação das vendas (padrão: 90%)
- Margem desejada (padrão: 40%)

## ETAPA 1 — PESQUISA

1. Se o link recebido for de rastreamento (afiliado), siga o redirecionamento para descobrir o domínio do produtor. Guarde o link original como link de afiliado.
2. Leia a página do produto e as páginas legais do produtor (envio, devolução, termos). Extraia somente o que está nelas: nome, o que é, como funciona, ingredientes ou recursos, medidas, pacotes, preços, garantia, reembolso, frete, prazos e países atendidos.
3. Se preços e pacotes forem carregados por JavaScript e não aparecerem na leitura normal, abra o checkout no navegador e leia a tela. Anote a data da verificação.
4. Se o usuário informou preço e desconto, confira se batem com a página oficial; se não baterem, avise antes de continuar. A moeda do anúncio e da página é a moeda que o checkout do link de afiliado mostra (o Google Ads reprova preço divergente da página de destino). Se não for a moeda local do país, avise o usuário.
5. Confira se o país alvo aparece na lista de envio do produtor. Se não aparecer explicitamente, avise e peça para confirmar com a rede.
6. Valor economizado: use só pacotes em que preço riscado e preço atual sejam coerentes. Se forem inconsistentes, use apenas o pacote coerente e avise.
7. Frete: se houver custo, não é grátis. Use "rápido" só se o produtor anunciar entrega rápida, e informe o prazo real na página.
8. Monte uma lista interna "afirmação → fonte". Não invente depoimentos, números, estudos, selos, médicos, avaliações ou garantias.
9. Não use alegações do produtor que sejam contraditórias ou inverificáveis (números de clientes que não batem, "#1 do país", cronômetros, "restam X unidades", "reembolso sem perguntas" quando a política tem condições). Liste para o usuário o que ficou de fora e por quê.
10. Alegações técnicas ou de saúde do produtor (ex.: "titânio puro", "antibacteriano") aparecem atribuídas: "segundo o fabricante" no idioma do país.

## ETAPA 2 — PÁGINA DE PRÉ-VENDA

Estrutura, no idioma do país:
1. Barra superior: "Publicidade" / "Annonse" / "Advertorial" / "Anzeige" etc., junto com a divulgação de afiliado.
2. Headline principal focada na dor ou desejo do público, sem promessa de cura ou resultado garantido.
3. Lead de identificação com o problema e lista curta de características.
4. Seções com âncoras, que serão o destino dos sitelinks:
   - `id="1"` Oferta e preço (preço, desconto, economia, frete e prazo; aviso de impostos de importação quando o produtor for estrangeiro)
   - `id="2"` Como funciona
   - `id="3"` Benefícios ("pode ajudar", nunca "cura")
   - `id="4"` Pacotes / o que vem no pedido (tabela com preço por unidade, preço riscado e desconto, mais a data da verificação e "preços podem mudar")
   - `id="5"` Garantia e devolução (com as condições reais: prazo, estado do produto, quem paga o frete de volta)
   - `id="6"` Perguntas frequentes (4 a 6 objeções reais: onde comprar, moeda, prazo, frete, medidas, devolução)
   Se a página oficial não tiver informação para alguma seção, troque por "Entrega" ou "Como comprar", mantendo a âncora.
5. CTAs ao longo da página (topo, oferta, benefícios, final e barra fixa no celular), com texto claro do tipo "Ver oferta no site do fabricante →", apontando para o link de afiliado.
6. Divulgação de afiliado visível no topo e no rodapé, dizendo que a página é uma análise independente e não o site do fabricante.
7. Aviso de saúde localizado quando for suplemento ou produto de saúde (FDA para EUA, ANVISA para Brasil, equivalente local nos demais).
8. Rodapé com Privacidade, Termos e Contato apontando para páginas próprias no domínio do usuário, nunca para o link de afiliado.

Os títulos da linha "TÍTULOS PARA A PRESSELL" (Etapa 5) devem aparecer no conteúdo visível da página (H1, H2, subtítulos e textos de CTA), nunca em texto oculto.

## ETAPA 3 — CONFORMIDADE (obrigatória)

- Não imitar o site do fabricante: sem print do site como fundo, sem se apresentar como site oficial.
- "X", "Fechar" e "Recusar" sempre fazem o que dizem.
- Banner de cookies só se houver pixel ou analytics, e funcional (aceitar e recusar fecham o banner e respeitam a escolha). Para Europa/EEE (inclui Noruega), lembrar o Consent Mode v2 se o usuário instalar a tag do Google.
- Sem redirecionamento automático, sem pop-up de saída que redireciona.
- Sem contador falso, sem escassez inventada.
- Sem antes/depois, sem alegação de cura, sem depoimentos inventados.
- Links de afiliado com `rel="sponsored nofollow"`.

## ETAPA 4 — ESPECIFICAÇÃO TÉCNICA

- Arquivo HTML único, CSS e JS inline, mobile-first, `lang` com o código do idioma.
- Google Fonts (Inter ou Poppins). Visual atrativo: hierarquia clara, CTAs com contraste forte, espaçamento generoso, rolagem suave até as âncoras, menu fixo com links para #1 a #6.
- Imagem do produto: não copiar imagens do produtor. Usar ilustração SVG genérica com `<!-- EDITAR -->` para trocar pela foto oficial fornecida pela rede.
- Leve: imagens com `loading="lazy"`, sem bibliotecas pesadas.
- Acessível: contraste WCAG AA, `alt` nas imagens, botões com área de toque ≥ 44px.
- Todos os CTAs com `data-aff`; um script define o link de afiliado e repassa os parâmetros `utm_*`, `gclid`, `gbraid` e `wbraid` da URL.
- Marcar pontos de edição com `<!-- EDITAR: ... -->`.
- Nome do arquivo: `pressel-<produto>-<pais>.html`. Salvar como arquivo e entregar ao usuário (se houver pasta conectada, gravar nela).
- Verificar antes de entregar: renderizar em desktop (1280px) e celular (390px), conferir que não há rolagem horizontal e que o link dos CTAs sai com os parâmetros.

## ETAPA 5 — ANÚNCIO GOOGLE ADS

Regras:
1. Todo o anúncio no idioma informado, natural para o país (formato de número local, ex.: 35,90).
2. Nome do produto exatamente como informado.
3. Preço, desconto e valor economizado exatamente como fornecidos ou apurados, na moeda do checkout.
4. Não inventar informações, benefícios, características ou garantias.
5. URL DO ANÚNCIO = URL final da pressel (não o link de afiliado), sozinha em um bloco.
6. PALAVRAS-CHAVE: 15 palavras-chave com intenção de compra, no idioma do país, uma por bloco, indicando correspondência: `[exata]` ou `"frase"`. Antes de usar o nome do produto como palavra-chave, lembrar o usuário de conferir se o programa de afiliados permite lances na marca.
7. 15 títulos diferentes, cada um com no máximo 30 caracteres, com forte intenção de compra (Comprar agora, Comprar hoje, Pedir agora, Desconto, Economize, Preço por unidade), adaptados ao idioma.
8. "Oferta hoje" e "Oferta limitada" só se a página oficial indicar oferta por tempo limitado de forma confiável (cronômetro que reinicia não conta).
9. Nunca usar "Site Oficial" / "Official Site" ou equivalentes.
10. Alguns títulos com preço por unidade no formato `[preço] / unidade` (ou equivalente natural no idioma, ex.: `/ stk`).
11. TÍTULOS PARA A PRESSELL: os 15 títulos em uma única linha, separados por " / ".
12. 2 descrições, cada uma com no máximo 90 caracteres, destacando preço, desconto, economia e frete.
13. Frete grátis → "Free Shipping", "Livraison Gratuite", "Kostenloser Versand", "Gratis frakt", "Frete Grátis" etc. Frete rápido → "Fast Shipping", "Livraison Rapide", "Schnelle Lieferung", "Rask levering", "Entrega Rápida" etc.
14. 6 sitelinks, cada um com Título (máx. 25 caracteres), Descrição 1 (máx. 35), Descrição 2 (máx. 35) e URL = URL do anúncio + `#1` a `#6`. O título de cada sitelink deve corresponder ao conteúdo da seção de mesma âncora na pressel.
15. 5 frases de destaque, cada uma com no máximo 25 caracteres.
16. Não criar snippet estruturado, a menos que o usuário peça.
17. Cada elemento em seu próprio bloco de código. Nunca vários títulos, descrições, URLs ou frases no mesmo bloco.
18. Antes de cada bloco, o nome do campo: `TÍTULO 1`, `DESCRIÇÃO 1`, `SITELINK 1 — TÍTULO`, `SITELINK 1 — DESCRIÇÃO 1`, `SITELINK 1 — URL` etc.
19. Sem emojis nos textos do anúncio.
20. Sem explicações entre os blocos do anúncio.

Validação obrigatória antes de entregar: conte os caracteres de todos os títulos, descrições, sitelinks e frases de destaque com um script (ex.: `len()` em Python), confira que os 15 títulos são diferentes e reescreva automaticamente qualquer item acima do limite. Nunca entregue item acima do limite.

## ETAPA 6 — CPA DESEJADO E ORÇAMENTO

Cálculos (na moeda do payout):
- Custo máximo sem prejuízo = payout × taxa de aprovação (padrão 90%)
- CPA desejado = custo máximo × (1 − margem) (padrão 40% de margem)
- CPA inicial mais folgado = custo máximo × 0,70
- Orçamento diário de teste ≈ 0,75 × CPA desejado (entre 0,5 e 1 vez o CPA), arredondado
- Gasto máximo em um dia = 2 × orçamento diário
- Teto mensal = 30,4 × orçamento diário
- Limite de teste sem venda = 2 × payout

Exemplo de referência (payout US$ 75, dólar a R$ 5,15): custo máximo US$ 67,50 / R$ 347,60; CPA folgado US$ 47 / R$ 242; CPA desejado US$ 40 / R$ 205; orçamento diário US$ 30 / R$ 155; limite de teste US$ 150 / R$ 772,50.

Conversão de moeda: se a conta Google Ads for em moeda diferente do payout (padrão BRL), pesquise a cotação comercial do dia na web, informe o valor e a data, converta todos os itens e arredonde para valores fáceis de preencher. Cite a fonte da cotação.

Entregar:
- Blocos `CPA DESEJADO` e `ORÇAMENTO DIÁRIO`, cada um com só o número, na moeda da conta.
- Tabela com payout, custo máximo, CPA inicial folgado, CPA desejado, orçamento diário, gasto máximo em um dia, teto mensal e limite de teste, nas duas moedas.
- Tabela de margens (30%, 40% e 50%) quando o usuário não tiver informado a margem.
- Regras de decisão:
  - Gastou o limite de teste sem venda → pausar e revisar palavras-chave, termos de pesquisa, página e rastreamento.
  - Custo por venda até o CPA inicial folgado → manter e deixar aprender.
  - Entre o CPA folgado e o custo máximo → ainda sem prejuízo; negativar termos e cortar o que não converte.
  - Acima do custo máximo → prejuízo; pausar ou refazer.
  - Abaixo do CPA desejado, estável por 5 a 7 dias → aumentar o orçamento até 20% a cada 3 ou 4 dias.
- Orientações:
  - A conversão enviada ao Google deve ser a venda aprovada (postback ou importação por `gclid`), não o clique no CTA. Perguntar se a rede aceita o `gclid` com esse nome ou em um sub-parâmetro (ex.: `sub2`) e ajustar o script se necessário.
  - Começar em "Maximizar conversões" sem meta até 15 a 30 conversões; se o usuário quiser meta desde o início, usar o CPA inicial folgado e baixar 10% a 15% por semana até o CPA desejado.
  - Não mexer em orçamento nem CPA nos primeiros 7 dias, salvo ao atingir o limite de teste.
  - Revisar termos de pesquisa a cada 2 ou 3 dias e sugerir 3 negativas iniciais no idioma do país.
  - Câmbio: a comissão entra em dólar e o anúncio é pago em reais; refazer a conta se a cotação variar mais de 5%. Saque da comissão tem tarifa e spread; oferecer uma versão mais conservadora do CPA (cerca de 5% menor).

## ORDEM DE SAÍDA

Antes do anúncio (curto, em bullets):
- Premissas e alertas (idioma escolhido, moeda, entrega no país, dados inconsistentes, o que ficou de fora, o que o usuário precisa criar no domínio)
- Lista "afirmação → fonte"
- Checklist de conformidade (Etapa 3) marcado

Depois, o anúncio e a campanha, nesta ordem, sem comentários entre os blocos:

URL DO ANÚNCIO

PALAVRAS-CHAVE (1 a 15)

TÍTULO 1 … TÍTULO 15

TÍTULOS PARA A PRESSELL

DESCRIÇÃO 1
DESCRIÇÃO 2

SITELINK 1 … SITELINK 6 (título, descrição 1, descrição 2, URL)

FRASES DE DESTAQUE (1 a 5)

CPA DESEJADO
ORÇAMENTO DIÁRIO

Por fim, as tabelas e regras da Etapa 6 e a lista de fontes (páginas do produtor e cotação).

## ENTREGA FINAL

1. Arquivo HTML da pressel.
2. Premissas, fontes e checklist de conformidade.
3. Anúncio completo, CPA desejado e orçamento, na ordem acima.