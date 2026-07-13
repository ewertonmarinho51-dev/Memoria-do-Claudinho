# Estratégia de RAG

O RAG deve ser uma biblioteca institucional controlada, não uma gaveta única com todos os PDFs.

## Camadas

1. **Corpus jurídico oficial:** Lei nº 14.133/2021 consolidada, normas federais aplicáveis, decretos e regulamentos.
2. **Corpus municipal:** Lei Orgânica, decretos, instruções normativas, portarias, fluxos e competências.
3. **Manuais institucionais:** orientações para DFD, ETP, TR, riscos e pesquisa de preços.
4. **Modelos aprovados:** utilizados para estrutura, linguagem e densidade.
5. **Memória do processo:** documentos e dados do processo atual.

## Regra decisiva

A memória do processo não deve depender de busca ampla. Quando o sistema sabe que um arquivo é o DFD do processo, ele deve anexá-lo diretamente ao contexto ou extrair campos estruturados. RAG é usado para localizar referências dentro de grandes corpora, não para redescobrir dados conhecidos.

## Busca

Utilizar recuperação híbrida:

- lexical/BM25 para artigos, expressões e identificadores;
- vetorial para semântica;
- reranking;
- filtros por órgão, tipo, vigência, documento e cláusula;
- diversidade para evitar dez trechos do mesmo parágrafo.
