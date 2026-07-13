# Riscos e limitações

| Risco | Impacto | Mitigação |
|---|---|---|
| confiar apenas na memória do modelo sobre legislação | conteúdo desatualizado ou impreciso | corpus jurídico oficial, versionado e atualizado |
| web aberta durante a geração | variabilidade, fonte não oficial, custo e latência | atualização jurídica assíncrona e fontes allowlisted |
| RAG sem metadados | contaminação entre órgãos, versões e tipos | filtros obrigatórios e trace de recuperação |
| uma chamada com saída excessiva | truncamento | perfis de tamanho, modelo adequado e validação de término |
| auditor ser o mesmo prompt do gerador | viés de autoaprovação | papel, instrução e contexto próprios; regras determinísticas |
| reescrever documento inteiro na correção | deriva | patch com hash e escopo permitido |
| aprovação automática interpretada como parecer | risco jurídico | relatório claro de natureza automatizada e assinatura competente |
| modelo antigo usado como fonte factual | resíduos de outro processo | separar referências de forma e fontes materiais |
| PDF por posicionamento absoluto | cortes e sobreposição | DOCX estruturado e conversão controlada |
