# Retrieval e Context Builder

## Retrieval plan por documento

O sistema mantém consultas declarativas por cláusula. Exemplo:

```json
{
  "documentType": "ETP",
  "clauseId": "etp.market.analysis",
  "queries": [
    "levantamento de mercado alternativas de solução",
    "justificativa técnica e econômica da solução escolhida"
  ],
  "filters": {
    "usageMode": ["LEGAL_AUTHORITY", "INSTITUTIONAL_GUIDANCE", "STYLE_REFERENCE"],
    "isActive": true
  },
  "topK": 8,
  "maxTokens": 5000
}
```

## Context Builder

O Context Builder:

- executa as consultas;
- aplica filtros obrigatórios;
- remove duplicatas;
- reranqueia;
- separa fato, norma, orientação e estilo;
- limita tokens por seção;
- preserva IDs das fontes;
- gera `retrievalCoverage`;
- falha quando fonte obrigatória não foi encontrada.

## Prova de consulta

A interface e os logs devem mostrar:

- quais fontes foram consultadas;
- quais trechos foram selecionados;
- qual cláusula usou cada trecho;
- por que um trecho foi incluído;
- score e filtros aplicados.
