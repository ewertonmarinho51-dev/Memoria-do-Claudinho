# Observabilidade

## Métricas

- gerações iniciadas, concluídas e falhas;
- tempo por etapa;
- tamanho da fila;
- custo e tokens por documento;
- falhas de RAG;
- políticas aplicadas;
- documentos bloqueados;
- erros de renderização;
- tentativas de acesso cruzado;
- latência e erros por tenant.

## Logs

Estruturados, com correlation ID, tenant, processo, documento, etapa e código de erro. Sem chave de API e sem conteúdo integral desnecessário.

## Alertas

- taxa de erro acima do limite;
- fila acumulada;
- banco ou storage indisponível;
- geração sem retrieval esperado;
- conversão falhando;
- aumento anormal de custo;
- violação de isolamento.
