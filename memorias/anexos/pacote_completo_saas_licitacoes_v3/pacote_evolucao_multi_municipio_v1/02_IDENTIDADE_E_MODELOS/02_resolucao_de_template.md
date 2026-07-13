# Resolução de template

## Entradas

- tenant;
- secretaria;
- tipo documental;
- categoria do objeto;
- modalidade ou hipótese;
- uso de registro de preços;
- versão das políticas;
- data de referência.

## Saída

O `TemplateResolver` retorna:

- template selecionado;
- versão;
- branding resolvido;
- cláusulas fixas;
- cláusulas condicionais;
- slots dinâmicos;
- slots de assinatura;
- regras aplicadas;
- alertas e conflitos.

## Determinismo

Com as mesmas entradas e versões, a resolução deverá produzir o mesmo resultado. A IA não participa da seleção do template nem da ativação das cláusulas.
