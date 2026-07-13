# Eventos de domínio

- `TenantCreated`;
- `TenantActivated`;
- `SecretariatCreated`;
- `BrandingProfilePublished`;
- `TemplateVersionPublished`;
- `ClauseVersionPublished`;
- `PolicyVersionPublished`;
- `InstitutionalAssignmentStarted`;
- `InstitutionalAssignmentEnded`;
- `InstitutionalContextResolved`;
- `PolicyAppliedToProcess`;
- `DocumentSignatoriesResolved`;
- `DocumentIssued`.

Eventos devem conter `tenant_id`, identificador, versão, ator, data e correlation ID. Consumidores devem ser idempotentes.
