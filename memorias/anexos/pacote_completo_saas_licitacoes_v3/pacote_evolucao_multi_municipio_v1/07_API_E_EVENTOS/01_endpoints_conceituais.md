# Endpoints conceituais

Adaptar à API atual.

```text
POST   /platform/tenants
GET    /platform/tenants/{tenantId}
POST   /platform/tenants/{tenantId}/secretariats
POST   /platform/tenants/{tenantId}/branding-profiles
POST   /platform/tenants/{tenantId}/templates
POST   /platform/tenants/{tenantId}/clauses
POST   /platform/tenants/{tenantId}/policies/simulate
POST   /platform/tenants/{tenantId}/policies/{id}/publish
POST   /platform/tenants/{tenantId}/people
POST   /platform/tenants/{tenantId}/assignments
GET    /processes/{processId}/signature-slots/{slotCode}/eligible-people
POST   /processes/{processId}/institutional-context/resolve
POST   /processes/{processId}/documents/preview
```

Todos os endpoints devem validar autorização e coerência de tenant no backend.
