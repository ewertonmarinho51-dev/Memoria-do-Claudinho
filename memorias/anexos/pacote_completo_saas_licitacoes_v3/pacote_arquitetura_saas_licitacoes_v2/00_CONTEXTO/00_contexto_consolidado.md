# Contexto consolidado

O SaaS já possui fluxo de geração de documentos, divisão por secretarias, identidade visual e regras de formatação. A alteração proposta não deve reconstruir o produto. Ela deve substituir ou encapsular o núcleo de geração e revisão que hoje produz documentos curtos, genéricos, possivelmente sem recuperar o RAG e com falhas de composição visual.

## Problemas relatados

- documentos muito menos densos que os modelos manuais;
- ausência de complexidade proporcional ao objeto;
- suspeita de que a chave da API ou o RAG não sejam efetivamente usados;
- formatação diferente do padrão institucional;
- trechos cortados, deslocados ou fora da área útil;
- tabelas e assinaturas quebradas;
- placeholders no documento final;
- revisão inicialmente manual;
- risco de uma nova geração alterar tudo, mesmo quando apenas uma cláusula precisa de correção.

## Necessidade do usuário final

O servidor público deve ser guiado por uma interface clara. O sistema deverá explicar quais informações faltam, por que são necessárias, de qual documento elas serão utilizadas e qual impacto têm na geração. O usuário não deverá conhecer engenharia de prompt, RAG ou modelos de IA.

## Objetivo funcional

Produzir DFD, ETP, Termo de Referência, Mapa de Riscos e demais artefatos com:

- estrutura institucional;
- profundidade compatível com o objeto;
- rastreabilidade de fontes;
- coerência entre documentos;
- conformidade normativa verificável;
- formatação idêntica aos modelos aprovados;
- revisão automática antes da emissão;
- correção incremental sem deriva documental.
