# Contratos de integração

## Endpoint conceitual de geração

`POST /processes/{processId}/documents/{documentType}/generate`

Retorna job, nunca PDF imediato.

## Eventos

- `DocumentGenerationRequested`;
- `ContextBuilt`;
- `DocumentGenerated`;
- `CanonicalValidationFailed`;
- `DocumentRendered`;
- `BundleAuditRequested`;
- `BundleAuditCompleted`;
- `PatchRequested`;
- `PatchApplied`;
- `DocumentApproved`;
- `DocumentIssued`.

## Idempotência

Toda operação mutável recebe `idempotencyKey`. Repetição da mesma requisição não cria nova versão sem necessidade.

## Adapters

Preservar a interface atual da aplicação e inserir adapters para:

- provider de modelo;
- provider de vector search;
- renderer;
- storage;
- workflow.
