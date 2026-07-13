# Manutenção assistida por IA

## Modelo seguro

A IA poderá:

- analisar logs e métricas;
- correlacionar falhas;
- abrir issue;
- sugerir correção;
- criar branch;
- preparar testes;
- abrir pull request;
- resumir risco e plano de rollback.

## A IA não deverá, inicialmente

- alterar produção diretamente;
- ignorar revisão de código;
- executar migration destrutiva sem aprovação;
- modificar segredos;
- desativar auditoria;
- publicar correção sem CI.

## Fluxo

`alerta -> triagem por IA -> reprodução -> patch em branch -> testes -> pull request -> revisão humana -> canário -> monitoramento -> promoção ou rollback`

Autonomia pode aumentar gradualmente para correções de baixo risco e reversíveis, sempre com política explícita.
