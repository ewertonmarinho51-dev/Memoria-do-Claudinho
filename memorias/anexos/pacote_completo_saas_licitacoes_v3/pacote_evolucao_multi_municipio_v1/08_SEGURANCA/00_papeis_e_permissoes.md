# Papéis e permissões

## Papéis iniciais

### PLATFORM_OWNER

Configura tenants, secretarias, modelos, políticas, cláusulas, identidades, feature flags e acessos globais.

### TENANT_ADMIN

Papel opcional, desabilitado inicialmente. Pode ser delegado no futuro com escopo limitado.

### SECRETARIAT_MANAGER

Gerencia equipe e processos da secretaria quando autorizado, sem publicar políticas globais.

### PUBLIC_SERVANT

Elabora processos e documentos dentro das unidades às quais está vinculado.

### AUDITOR / REVIEWER

Consulta trilhas, relatórios e documentos conforme permissão.

## Regras

- negar por padrão;
- menor privilégio;
- escopo por tenant e secretaria;
- ações sensíveis exigem reautenticação ou confirmação;
- publicação e rollback geram trilha de auditoria.
