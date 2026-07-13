# Ingestão, chunking e metadados

## Pipeline de ingestão

1. receber arquivo;
2. calcular hash;
3. identificar tipo e origem;
4. extrair texto e estrutura;
5. preservar títulos, artigos, cláusulas e páginas;
6. detectar versão, vigência e revogação;
7. dividir por unidade semântica;
8. gerar embeddings;
9. indexar texto e metadados;
10. executar testes de recuperação.

## Chunking recomendado

- leis: por artigo, parágrafo, inciso e alínea, mantendo caminho hierárquico;
- manuais: por seção e subtítulo;
- modelos: por cláusula e subcláusula;
- tabelas: unidade completa, com cabeçalho repetido no texto;
- processo atual: por documento e campo estruturado.

Evitar chunks arbitrários que cortem artigo ou cláusula.

## Metadados mínimos

- `sourceId`;
- `title`;
- `sourceType`;
- `usageMode`;
- `jurisdiction`;
- `organizationId`;
- `departmentId`;
- `documentType`;
- `sectionPath`;
- `pageStart`, `pageEnd`;
- `effectiveFrom`, `effectiveTo`;
- `version`;
- `officialSource`;
- `contentHash`;
- `confidentiality`;
- `processId`, quando aplicável.
