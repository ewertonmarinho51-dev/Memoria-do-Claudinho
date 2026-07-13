# Pacote de Arquitetura para Geração e Auditoria de Documentos de Licitações

Versão: 2.0  
Data de consolidação: 12/07/2026  
Finalidade: orientar a implementação incremental, no sistema SaaS já existente, de uma geração documental previsível, fundamentada, auditável e com correções localizadas.

## Resultado esperado

O sistema deverá orientar o servidor público durante o planejamento, montar o contexto do processo, recuperar apenas referências pertinentes, gerar cada documento em uma única chamada principal ao modelo, auditar o conjunto de documentos e aplicar correções como patches controlados.

O fluxo nominal é:

1. coleta guiada e validação dos dados;
2. montagem determinística do contexto;
3. recuperação híbrida no RAG;
4. uma chamada de geração por documento;
5. validações determinísticas;
6. renderização em DOCX e PDF;
7. uma chamada de auditoria do conjunto;
8. emissão, quando aprovado;
9. uma chamada de patch consolidado, apenas quando houver reprovação;
10. nova validação sem reescrever trechos não afetados.

## Decisões centrais

- O modelo não recebe liberdade para reconstruir a arquitetura do documento a cada ciclo.
- O documento canônico é JSON estruturado e versionado. DOCX e PDF são projeções desse JSON.
- O RAG fornece legislação local, normas, manuais institucionais e padrões aprovados.
- Os dados do processo atual são fornecidos diretamente ao gerador, não recuperados por semelhança quando já são conhecidos.
- A Lei nº 14.133/2021 e demais normas críticas devem existir em corpus jurídico oficial, consolidado, versionado e auditável. O sistema não deve depender apenas da memória do modelo ou de pesquisa aberta na internet.
- O auditor não reescreve documentos. Ele emite achados estruturados.
- O corretor aplica somente as alterações autorizadas pelo relatório de auditoria.
- Regras objetivas, cálculos, numeração, formatação e geometria de página são validados por código.
- A aprovação automática é uma decisão de produto e não equivale a certificação jurídica. A assinatura e a responsabilidade administrativa permanecem com os agentes competentes.

## Como usar este pacote

1. Entregue o arquivo `MASTER_PROMPT_IMPLEMENTACAO.md` à IA com acesso ao repositório.
2. Disponibilize este diretório completo no workspace da IA.
3. Exija que ela audite o código existente antes de alterar qualquer arquivo.
4. Exija implementação incremental por feature flags e testes de regressão.
5. Não aceite uma resposta apenas teórica. A IA deverá alterar o código, executar testes e documentar as evidências.

## Navegação

- `00_CONTEXTO`: objetivos, princípios e glossário.
- `01_DIAGNOSTICO`: problemas observados e métricas dos documentos.
- `02_ARQUITETURA`: pipeline, estados, versionamento e orçamento de chamadas.
- `03_RAG`: ingestão, busca, filtros, segurança e atualização jurídica.
- `04_PROMPTS`: prompts de produção para cada papel do sistema.
- `05_SCHEMAS`: contratos JSON para geração, auditoria e patches.
- `06_PERFIS_DOCUMENTAIS`: complexidade e estrutura esperadas.
- `07_VALIDACOES`: regras determinísticas, jurídicas e visuais.
- `08_UX`: experiência guiada do usuário.
- `09_IMPLEMENTACAO`: adoção incremental no repositório existente.
- `10_TESTES`: estratégia, casos de aceitação e dataset de avaliação.
- `11_EXEMPLOS`: exemplos de payloads e resultados.
- `12_CONFIG`: configurações de referência.
- `13_REFERENCIAS`: métricas e notas técnicas.

## Limite de escopo

Este pacote não substitui a análise do repositório real. Nomes de arquivos, frameworks, banco de dados, filas, serviços e componentes deverão ser adaptados à arquitetura encontrada, preservando funcionalidades já existentes.
