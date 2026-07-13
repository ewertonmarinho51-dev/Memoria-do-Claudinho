# Casos de aceitação

1. API ausente bloqueia sem fallback.
2. RAG vazio bloqueia cláusula jurídica obrigatória.
3. DFD complexo atinge profundidade adequada sem repetição.
4. ETP contém alternativas reais e justificativa de escolha.
5. TR reflete integralmente a solução do ETP.
6. Mapa de Riscos gera controles no TR.
7. Quantidade divergente gera finding alto.
8. Valor incorreto é detectado por código.
9. Fonte revogada não é usada.
10. Modelo histórico não fornece fato.
11. Prompt injection em PDF é ignorada.
12. Resposta truncada é rejeitada.
13. JSON inválido é rejeitado.
14. Placeholder bloqueia emissão.
15. Fonte Helvetica no corpo bloqueia.
16. Texto fora da página bloqueia.
17. DOCX e PDF divergentes bloqueiam.
18. Auditor aprovado permite emissão.
19. Patch altera somente cláusula autorizada.
20. Patch com hash antigo falha.
21. Mudança de dado material marca dependentes como `STALE`.
22. Tenant A não recupera fonte do tenant B.
23. Regeneração idempotente não cria duplicata.
24. Rollback preserva documentos emitidos.
25. Logs comprovam modelo, prompt e retrieval sem expor segredo.
