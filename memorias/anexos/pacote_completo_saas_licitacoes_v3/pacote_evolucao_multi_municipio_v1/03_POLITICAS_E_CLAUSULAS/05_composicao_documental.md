# Composição documental

## Pipeline

1. resolver contexto institucional;
2. resolver template e versão;
3. avaliar políticas;
4. selecionar cláusulas fixas e condicionais;
5. identificar slots geráveis;
6. gerar apenas conteúdo variável;
7. validar JSON canônico;
8. compor todos os blocos;
9. resolver numeração e referências;
10. validar conteúdo e coerência;
11. renderizar DOCX e PDF;
12. auditar conjunto.

## Garantias

- cláusula fixa preserva hash;
- nenhum slot fica sem conteúdo;
- numeração é calculada após composição;
- referências internas são resolvidas por IDs, não por números escritos manualmente;
- a ordem final é definida pelo template.
