# Fontes e prioridades

## Prioridade material

1. campos confirmados do processo atual;
2. documento inicial da demanda;
3. anexos do processo atual;
4. documentos canônicos anteriores do mesmo processo;
5. legislação oficial vigente;
6. regulamentação municipal;
7. manuais institucionais;
8. modelos aprovados, apenas para forma.

## Tipos de uso

Cada fonte deve declarar `usageMode`:

- `MATERIAL_FACT`: pode fornecer fato do processo;
- `LEGAL_AUTHORITY`: pode fornecer fundamento normativo;
- `INSTITUTIONAL_GUIDANCE`: pode orientar procedimento;
- `STYLE_REFERENCE`: pode orientar forma, nunca fato;
- `PROHIBITED_FOR_GENERATION`: disponível apenas para auditoria ou histórico.

## Conflitos

Conflitos materiais não resolvidos por prioridade devem bloquear. O modelo não escolhe silenciosamente um valor.
