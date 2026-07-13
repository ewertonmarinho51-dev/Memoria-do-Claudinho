# Hospedagem e escala

## Ponto de partida recomendado

Manter monólito modular e serviços gerenciados, se essa for a arquitetura atual:

- aplicação web/API stateless;
- banco relacional gerenciado;
- armazenamento de objetos;
- fila para geração, conversão e auditoria;
- workers separados para tarefas pesadas;
- cache opcional;
- observabilidade centralizada;
- backups automatizados.

## Por que não começar com microserviços

Quinze secretarias e dezenas ou centenas de usuários não justificam, por si só, complexidade operacional de microserviços. Separar módulos e filas é suficiente. Decomposição física vem após métricas reais.

## Escala

- web/API escala horizontalmente;
- workers escalam conforme tamanho da fila;
- limites de concorrência protegem API de IA e conversor;
- jobs são idempotentes;
- arquivos são externos ao container;
- banco usa pool de conexões;
- operações longas não prendem requisições HTTP.
