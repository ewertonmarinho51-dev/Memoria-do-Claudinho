# Orquestração e orçamento de chamadas

## Caminho nominal recomendado

| Fase | Chamadas ao modelo |
|---|---:|
| geração do DFD | 1 |
| geração do ETP | 1 |
| geração do TR | 1 |
| geração do Mapa de Riscos | 1 |
| auditoria consolidada | 1 |
| correção | 0 quando aprovado |

Total nominal para quatro documentos: 5 chamadas.

## Caminho de correção

Quando houver reprovação:

- uma chamada de patch consolidado pode corrigir todos os documentos afetados;
- o patch é aplicado aos JSONs canônicos;
- validações e renderização são refeitas sem chamada ao modelo;
- uma segunda auditoria só ocorre quando findings semânticos relevantes permanecerem.

## Por que não usar internet dentro de cada geração

A API não possui acesso automático à internet. A pesquisa web depende de ferramenta explicitamente habilitada. Mesmo quando habilitada, aumenta variabilidade e pode selecionar fontes inadequadas. Para conformidade, o caminho principal deve usar corpus oficial versionado. Pesquisa web deve ficar no processo de atualização do corpus, com allowlist e aprovação.

## Estratégia de uma chamada por documento

- retrieval ocorre antes da chamada;
- contexto é compactado e ordenado;
- Structured Output garante schema;
- não permitir tool calls no gerador principal;
- definir limite de saída compatível com o perfil;
- falhar quando o término indicar truncamento;
- não fazer retries invisíveis que possam gerar versões diferentes.
