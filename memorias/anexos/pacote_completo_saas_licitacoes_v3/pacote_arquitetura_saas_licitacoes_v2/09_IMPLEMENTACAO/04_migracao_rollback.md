# Migração e rollback

## Migração

- manter documentos legados imutáveis;
- criar versão canônica apenas para novas gerações;
- opcionalmente importar documento legado como `LEGACY_SNAPSHOT`;
- não reemitir arquivos antigos sem solicitação;
- guardar relação entre IDs antigos e novos.

## Rollback

- desativar flags;
- preservar versões produzidas;
- não apagar trilhas de auditoria;
- reverter migrations compatíveis ou usar migrations aditivas;
- impedir que documentos aprovados desapareçam.

## Compatibilidade

A interface pública e os links existentes devem continuar funcionando durante a transição.
