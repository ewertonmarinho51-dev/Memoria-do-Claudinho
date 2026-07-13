# Validação de formatação e renderização

## DOCX

- A4;
- margens do perfil;
- Times New Roman 12 no corpo;
- 1,5 entre linhas;
- 6 pt após parágrafo;
- estilos nomeados;
- títulos em negrito e `keepWithNext`;
- tabelas dentro da largura;
- cabeçalho e rodapé do template;
- numeração automática;
- assinaturas não separadas.

## PDF

- dimensões corretas;
- nenhuma caixa de texto fora da página;
- sem sobreposição;
- sem glyphs quebrados;
- sem páginas vazias inesperadas;
- tabelas legíveis;
- cabeçalho e rodapé sem colisão;
- texto extraído equivalente ao DOCX;
- fonte não substituída.

## Método

1. inspecionar DOCX internamente;
2. converter em ambiente padronizado;
3. extrair bounding boxes do PDF;
4. renderizar páginas para imagem;
5. executar regras geométricas;
6. comparar com golden snapshots tolerantes a pequenas diferenças;
7. opcionalmente usar visão para anomalias não capturadas.
