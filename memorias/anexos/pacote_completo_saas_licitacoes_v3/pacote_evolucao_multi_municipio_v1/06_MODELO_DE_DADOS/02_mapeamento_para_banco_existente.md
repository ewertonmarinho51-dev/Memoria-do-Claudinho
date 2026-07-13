# Mapeamento para o banco existente

A IA implementadora deverá mapear estas entidades para o modelo real, evitando duplicação.

## Estratégia

- reutilizar tabelas de organizações e usuários, se existentes;
- adicionar `tenant_id` e `secretariat_id` de forma nullable na primeira migration;
- criar tenant padrão para os dados atuais;
- fazer backfill;
- adicionar constraints somente após validação;
- preservar IDs e relações;
- criar views ou adaptadores temporários para o fluxo legado;
- não migrar arquivos em massa sem plano reversível.

## Entrega obrigatória

Matriz `entidade proposta | tabela existente | decisão | migration | compatibilidade`.
