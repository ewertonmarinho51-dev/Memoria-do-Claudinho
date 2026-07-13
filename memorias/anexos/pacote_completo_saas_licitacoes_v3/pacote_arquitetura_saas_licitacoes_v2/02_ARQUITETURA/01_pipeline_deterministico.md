# Pipeline determinístico

## Etapa 1: congelar a entrada

Ao iniciar a geração, o sistema cria um `inputSnapshot` com hash. Mudanças posteriores no formulário não alteram a geração em andamento.

## Etapa 2: construir contexto

O Context Builder produz um `ContextEnvelope` contendo:

- dados do processo;
- pendências resolvidas;
- documentos anteriores do mesmo processo;
- trechos jurídicos recuperados;
- trechos institucionais recuperados;
- perfil do documento;
- regras de formatação;
- restrições e incompatibilidades;
- orçamento de saída.

## Etapa 3: gerar

Uma chamada ao modelo retorna JSON em schema rígido. O modelo não gera DOCX ou PDF.

## Etapa 4: validar

O schema, a numeração, os campos, as citações, os cálculos e os limites de tamanho são validados por código.

## Etapa 5: renderizar

O documento canônico é aplicado a um template DOCX. A conversão para PDF ocorre em ambiente com fonte e versão de renderer controladas.

## Etapa 6: auditar o bundle

O auditor recebe os documentos canônicos, resultados determinísticos e evidências. Ele não recebe autorização para reescrever.

## Etapa 7: corrigir

O corretor recebe apenas findings aprovados, caminhos permitidos e versões-base. A saída é um `PatchPlan` validado.
