# Testes de isolamento obrigatórios

1. usuário do tenant A não lista processos do tenant B;
2. ID conhecido do tenant B retorna acesso negado;
3. worker não processa payload sem tenant;
4. cache não retorna resultado de outro tenant;
5. busca vetorial respeita escopo;
6. URL assinada não abre arquivo de outro tenant;
7. pessoa de outra secretaria não aparece em slot restrito;
8. log e relatório administrativo respeitam autorização;
9. exportação contém somente dados autorizados;
10. teste concorrente não mistura contexto entre requisições.
