# Isolamento de dados

## Banco de dados

- `tenant_id` não nulo em tabelas de domínio;
- índices compostos iniciados por `tenant_id` quando houver ganho;
- escopo automático no ORM ou repositórios;
- políticas de Row-Level Security quando a stack e o banco suportarem;
- testes negativos de acesso cruzado;
- jobs e workers sempre recebem `tenant_id` no payload.

## Arquivos

Estrutura sugerida:

```text
/tenants/{tenant_id}/secretariats/{secretariat_id}/processes/{process_id}/...
```

URLs de download devem ser temporárias e autorizadas no backend.

## RAG

- coleção separada por tenant ou filtros mandatórios por `tenant_id`;
- corpus nacional pode ser compartilhado em coleção somente leitura;
- corpus institucional deve ser isolado;
- cache de retrieval deve incluir tenant, versão e filtros na chave;
- nenhum exemplo histórico de outro município é recuperado sem autorização explícita.

## Logs

Logs técnicos podem carregar identificadores opacos. Não registrar conteúdo integral, segredos ou documentos pessoais sem necessidade.
