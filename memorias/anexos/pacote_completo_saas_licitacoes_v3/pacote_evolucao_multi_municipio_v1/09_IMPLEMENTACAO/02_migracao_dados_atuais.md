# Migração dos dados atuais

## Estratégia sem ruptura

1. criar tenant padrão representando o município atual;
2. mapear secretarias existentes;
3. associar usuários e processos;
4. converter configurações visuais em perfis versionados;
5. importar textos fixos atuais para catálogo em estado `DRAFT`;
6. revisar e publicar apenas após validação;
7. converter assinaturas livres em pessoas e vínculos;
8. manter adaptador para leitura do formato legado;
9. ativar novo resolver por feature flag;
10. comparar resultados antes de desligar o legado.

## Proibição

Não apagar colunas ou arquivos legados na mesma release que introduz o novo modelo.
