# Observabilidade

## Trace por geração

Registrar:

- `traceId`, `processId`, `documentId`;
- versão de entrada;
- rota e serviço;
- modelo e parâmetros;
- prompt version;
- IDs e scores dos trechos recuperados;
- tokens;
- latência;
- motivo de término;
- validação de schema;
- hash da saída;
- findings determinísticos;
- status final.

## Métricas

- taxa de documentos aprovados na primeira geração;
- findings por categoria;
- taxa de truncamento;
- cobertura de fontes;
- precisão do retrieval;
- custo por tipo documental;
- tempo por etapa;
- patches rejeitados por alteração fora de escopo;
- divergências entre DOCX e PDF;
- incidentes de fallback.

## Privacidade

Nunca registrar chave, token, credencial, documento integral desnecessário ou dado pessoal sem finalidade.
