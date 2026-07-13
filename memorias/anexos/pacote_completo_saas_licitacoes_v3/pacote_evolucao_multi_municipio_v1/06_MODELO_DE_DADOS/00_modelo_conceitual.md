# Modelo conceitual

## Núcleo organizacional

- `Tenant`;
- `Secretariat`;
- `UserMembership`;
- `InstitutionalPerson`;
- `InstitutionalAssignment`;
- `DocumentRole`;
- `PersonRoleAssignment`.

## Configuração visual

- `BrandingProfile`;
- `BrandingAsset`;
- `BrandingOverride`.

## Composição documental

- `DocumentTemplate`;
- `DocumentTemplateVersion`;
- `TemplateBlock`;
- `ClauseDefinition`;
- `ClauseVersion`;
- `PolicyRule`;
- `PolicyRuleVersion`;
- `SignatureSlotDefinition`.

## Execução

- `Process`;
- `Document`;
- `DocumentVersion`;
- `ResolvedInstitutionalContext`;
- `AppliedPolicy`;
- `ResolvedClause`;
- `DocumentSignatory`;
- `AuditEvent`.

Todas as entidades de domínio devem possuir identificador estável, tenant quando aplicável, timestamps, estado e metadados de auditoria.
