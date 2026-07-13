# Notas sobre capacidades da API

## Pesquisa web

Modelos de API não possuem pesquisa web automática em toda chamada. Na Responses API, a pesquisa precisa ser habilitada como ferramenta. Para o caminho crítico de geração jurídica, este pacote recomenda não depender de pesquisa aberta em tempo real.

Referência oficial: https://developers.openai.com/api/docs/guides/tools-web-search

## File search

A ferramenta de file search pode recuperar informações de arquivos armazenados em vector stores. Ainda assim, a aplicação deve registrar o retrieval trace e controlar filtros, cobertura e uso das fontes.

Referência oficial: https://developers.openai.com/api/docs/guides/tools-file-search

## Structured Outputs

Structured Outputs permite exigir aderência a JSON Schema e deve ser preferido a parsing de texto livre.

Referência oficial: https://developers.openai.com/api/docs/guides/structured-outputs

## Decisão deste pacote

- Structured Outputs no gerador, auditor e corretor;
- retrieval determinístico antes da geração;
- pesquisa web apenas no pipeline de atualização de corpus, quando aprovada;
- sem tool calls abertas no gerador principal.
