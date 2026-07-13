# Feature flags e rollback

## Flags sugeridas

- `MULTI_TENANT_CONTEXT_V1`;
- `SECRETARIAT_CONTEXT_V1`;
- `BRANDING_RESOLVER_V1`;
- `VERSIONED_TEMPLATES_V1`;
- `INSTITUTIONAL_PEOPLE_V1`;
- `SIGNATURE_ELIGIBILITY_V1`;
- `CLAUSE_CATALOG_V1`;
- `POLICY_ENGINE_SHADOW_V1`;
- `POLICY_ENGINE_ENFORCE_V1`;
- `INSTITUTIONAL_CONTEXT_SNAPSHOT_V1`.

## Rollback

- manter fluxo legado;
- migrations expand/contract;
- flags por tenant;
- snapshots de configuração;
- versões publicadas não são destruídas;
- documentação de reversão por fase.
