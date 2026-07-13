# Matriz de dependências documentais

| Documento | Depende de | Fornece para |
|---|---|---|
| Documento inicial / memorando | demanda do setor | DFD |
| DFD | demanda, planejamento, dados do setor | ETP, PCA, autorização |
| ETP | DFD, pesquisa, requisitos, alternativas | TR, mapa de riscos |
| Mapa de Riscos | DFD, ETP, histórico do objeto | TR, edital, contrato |
| TR | DFD, ETP, riscos, pesquisa de preços | edital, contrato |
| Edital | TR, pareceres, modalidade, regras locais | sessão e contratação |
| Contrato / ata | edital, TR, proposta vencedora | execução e fiscalização |

## Regra de consistência

Uma alteração em dado material deverá gerar uma lista determinística de documentos potencialmente impactados. O sistema deverá marcar esses documentos como `STALE` até nova validação ou patch.
