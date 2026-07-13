# Máquina de estados

Estados sugeridos:

```text
DRAFT
INPUT_VALIDATING
INPUT_BLOCKED
CONTEXT_BUILDING
CONTEXT_READY
GENERATION_IN_PROGRESS
GENERATION_FAILED
CANONICAL_VALIDATING
CANONICAL_REJECTED
CANONICAL_READY
DOCX_RENDERING
DOCX_REJECTED
PDF_RENDERING
PDF_REJECTED
READY_FOR_BUNDLE_AUDIT
BUNDLE_AUDITING
AUDIT_REJECTED
PATCH_PENDING
PATCH_IN_PROGRESS
PATCH_REJECTED
APPROVED_FOR_ISSUANCE
ISSUED
STALE
SUPERSEDED
```

## Regras

- transições ocorrem por eventos persistidos;
- nenhum estado de aprovação pode ser setado diretamente pela interface;
- documento alterado após aprovação recebe `STALE`;
- falha de API não pode avançar para renderização;
- documento com finding alto ou crítico não pode ser emitido;
- cada transição guarda ator, versão, timestamp e evidência.
