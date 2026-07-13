# Avaliação do RAG

## Dataset

Criar perguntas de referência para cada documento e cláusula, com trechos esperados.

## Métricas

- Recall@K de fontes corretas;
- precisão dos trechos;
- cobertura das cláusulas;
- contaminação por processo errado;
- uso de fontes revogadas;
- taxa de fontes sem validade;
- resposta sem evidência;
- latência e custo.

## Testes obrigatórios

- recuperar artigo específico;
- recuperar regulamento municipal correto;
- não recuperar modelo de outra secretaria quando filtrado;
- não usar documento revogado;
- distinguir estilo de fato;
- bloquear mistura entre processos;
- recuperar requisitos por tipo de objeto.

A aprovação do RAG deve ocorrer antes de avaliar a qualidade do prompt. Um prompt excelente não corrige retrieval ausente.
