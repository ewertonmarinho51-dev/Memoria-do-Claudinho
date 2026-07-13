# Regras de integridade

1. Toda entidade municipal deve possuir `tenant_id`.
2. Secretaria deve pertencer ao mesmo tenant da pessoa, processo e documento.
3. Vínculos não podem ter intervalos inválidos.
4. Não pode haver dois titulares ativos para papel exclusivo, salvo configuração expressa.
5. Versões publicadas de cláusulas e templates são imutáveis.
6. Política publicada referencia apenas cláusulas existentes e compatíveis.
7. Documento emitido deve possuir snapshot resolvido.
8. Signatário deve ter vínculo válido na data de referência.
9. Cláusula bloqueada deve manter o hash publicado.
10. Exclusão lógica deve preservar histórico e auditoria.
