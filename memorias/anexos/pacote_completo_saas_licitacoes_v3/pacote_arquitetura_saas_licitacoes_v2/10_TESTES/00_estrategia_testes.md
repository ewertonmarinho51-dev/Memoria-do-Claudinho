# Estratégia de testes

## Pirâmide

1. unitários para regras e normalização;
2. contratos para schemas e providers;
3. integração para RAG, geração, storage e renderer;
4. end-to-end para fluxo completo;
5. golden tests para documentos;
6. evals para qualidade semântica;
7. segurança e isolamento.

## Testes não determinísticos

- usar dataset fixo;
- temperatura mínima compatível;
- avaliar critérios, não texto idêntico;
- guardar modelo e prompt version;
- repetir amostra quando houver variância;
- bloquear regressão estatisticamente relevante.
