# Modelos documentais por município

## Modelo não é apenas um arquivo DOCX

Um modelo versionado deverá combinar:

- tipo documental;
- estrutura de seções;
- posições e âncoras;
- perfil de formatação;
- identidade visual resolvida;
- cláusulas fixas;
- slots gerados por IA;
- slots condicionais;
- slots de tabela;
- slots de assinatura;
- regras de paginação;
- versão e vigência.

## Tipos de bloco

- `FIXED_LOCKED`: texto aprovado, inserido sem reescrita;
- `AI_GENERATED`: conteúdo variável produzido pelo gerador;
- `CONDITIONAL_LOCKED`: cláusula fixa ativada por política;
- `DATA_BOUND`: valor proveniente de campo estruturado;
- `COMPUTED`: cálculo determinístico;
- `SIGNATURE_SLOT`: assinatura por papel;
- `TABLE_SLOT`: tabela composta por dados.

## Versionamento

Alterar um modelo cria nova versão. Processos já iniciados podem permanecer na versão anterior ou migrar de forma explícita e auditada.
