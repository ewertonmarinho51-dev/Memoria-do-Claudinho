# Feature flags

Flags sugeridas:

- `canonical_documents_v2`;
- `deterministic_context_builder`;
- `rag_trace_v2`;
- `structured_generation_v2`;
- `docx_renderer_v2`;
- `pdf_preflight_v2`;
- `bundle_auditor_shadow`;
- `bundle_auditor_gate`;
- `incremental_patch_v2`;
- `automatic_issuance_gate`.

## Regras

- flags por ambiente, tenant e tipo documental;
- rollback sem migração destrutiva;
- métricas comparativas entre legado e novo;
- ativação gradual;
- nenhum flag deve permitir burlar segurança ou isolamento.
