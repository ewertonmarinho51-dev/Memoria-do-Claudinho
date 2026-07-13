# Plano incremental

## Fase 0 — Prova e proteção

- testes de regressão do fluxo atual;
- métricas;
- feature flags;
- backup e rollback.

## Fase 1 — Tenant padrão

- criar tenant para o município atual;
- adicionar escopo sem mudar a UX;
- backfill;
- testes de isolamento.

## Fase 2 — Secretarias e contexto

- normalizar secretarias;
- vincular usuários;
- resolver contexto automaticamente.

## Fase 3 — Identidade e modelos

- perfil municipal;
- sobrescrita por secretaria;
- versionamento;
- preview e renderização.

## Fase 4 — Pessoas e assinaturas

- cadastro institucional;
- vínculos;
- slots e elegibilidade;
- migração dos nomes atuais.

## Fase 5 — Cláusulas e políticas

- catálogo;
- publicação;
- simulador;
- composição determinística.

## Fase 6 — Integração com motor documental v2

- snapshot institucional;
- geração de slots variáveis;
- auditoria e patches;
- emissão.

## Fase 7 — Segundo tenant piloto

- configurar município fictício ou sandbox;
- provar isolamento e flexibilidade;
- somente depois habilitar município real adicional.
