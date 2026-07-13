# Motor de políticas documentais

## Objetivo

Aplicar automaticamente cláusulas, blocos, validações e requisitos conforme fatos estruturados do processo e configurações do município.

## Exemplo conceitual

```text
SE modalidade = PREGÃO_ELETRÔNICO
E sistema_registro_precos = verdadeiro
E categoria_objeto = BENS
ENTÃO incluir CLAUSULA_X na posição Y
E executar VALIDACAO_Z
```

A regra jurídica concreta e o texto da cláusula devem ser cadastrados e aprovados pelo proprietário da plataforma com base em fonte oficial ou modelo institucional.

## O que não fazer

- não esconder políticas em prompts;
- não usar `if` espalhado pelo código;
- não permitir execução de código arbitrário cadastrado pelo administrador;
- não pedir ao servidor que lembre ou escolha cláusulas;
- não permitir que a IA decida sozinha se uma cláusula obrigatória se aplica.

## Linguagem de regras

Usar DSL declarativa e limitada, com operadores permitidos:

- igualdade e desigualdade;
- inclusão em lista;
- comparação numérica;
- presença de valor;
- `AND`, `OR`, `NOT`;
- datas e vigência.
