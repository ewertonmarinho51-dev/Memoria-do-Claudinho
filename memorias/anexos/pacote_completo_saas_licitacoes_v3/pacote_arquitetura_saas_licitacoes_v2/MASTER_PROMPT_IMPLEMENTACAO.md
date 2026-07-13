# PROMPT MESTRE PARA ANÁLISE E IMPLEMENTAÇÃO NO REPOSITÓRIO EXISTENTE

Você recebeu o pacote `pacote_arquitetura_saas_licitacoes_v2`. Ele descreve uma proposta de evolução do fluxo de geração, RAG, auditoria, correção e renderização de documentos de planejamento de contratações públicas.

O sistema já está em desenvolvimento e possui funcionalidades ativas. Sua tarefa não é reescrevê-lo do zero. Você deverá inspecionar o repositório, comparar a proposta com a arquitetura real, identificar o que é compatível, adaptar o que for necessário e implementar incrementalmente as melhorias úteis.

## 1. Regras de atuação

1. Analise todo o repositório relevante antes de alterar código.
2. Preserve funcionalidades, identidade visual, divisão por secretarias, autenticação, permissões, dados e integrações existentes.
3. Não imponha framework, banco, fila ou biblioteca nova sem demonstrar que a solução atual não atende.
4. Não renomeie ou mova módulos amplamente usados sem necessidade.
5. Faça mudanças pequenas, rastreáveis e cobertas por testes.
6. Use feature flags para introduzir o novo fluxo.
7. Não exclua o fluxo legado antes de comprovar paridade e permitir rollback.
8. Não entregue apenas análise ou pseudocódigo. Implemente, teste e documente.
9. Não use fallback silencioso quando a API ou o RAG falharem.
10. Não coloque segredos no frontend, logs, commits ou documentação.

## 2. Primeira etapa obrigatória: auditoria do sistema atual

Mapeie e documente:

- stack e organização do repositório;
- fluxo iniciado pelo botão de geração;
- frontend, backend, rotas, serviços, workers e filas envolvidos;
- local de leitura da chave da API;
- provider, modelo, parâmetros e limite de saída;
- prompts existentes;
- chamadas reais à API;
- tratamento de erro e fallback;
- RAG, vector store, embeddings, chunking e filtros;
- evidência de que o retrieval é chamado na rota ativa;
- dados enviados ao modelo;
- parsing da resposta;
- geração de DOCX e PDF;
- fontes instaladas no ambiente;
- armazenamento e versionamento;
- regras de multi-tenant e secretaria;
- testes, CI, logs e monitoramento.

Produza uma matriz com as colunas:

`componente | implementação atual | problema | reutilizar/adaptar/substituir | justificativa | risco | arquivos envolvidos`

Não comece uma reescrita antes dessa matriz.

## 3. Validação da proposta

Leia todos os arquivos deste pacote. Para cada componente proposto, classifique:

- compatível e reutilizável;
- compatível com adaptação;
- incompatível com a arquitetura atual;
- já existente;
- desnecessário;
- deve ser adiado.

Quando houver incompatibilidade, preserve o objetivo e adapte a forma de implementação à stack real.

## 4. Arquitetura funcional a implementar

O fluxo alvo deve ser:

`coleta guiada -> snapshot de entrada -> Context Builder -> retrieval rastreável -> uma chamada principal por documento -> JSON canônico -> validações por código -> DOCX -> PDF -> preflight -> auditoria consolidada -> emissão ou patch incremental`

### 4.1 Uma chamada por documento

No caminho nominal, realize no máximo uma chamada principal ao modelo para gerar cada documento.

Antes da chamada:

- recupere as fontes por código;
- aplique filtros;
- compacte o contexto;
- registre retrieval trace;
- inclua o perfil documental;
- inclua dados confirmados do processo;
- inclua documentos predecessores canônicos.

Durante a chamada:

- use Structured Output com schema;
- não habilite pesquisa web aberta;
- não permita que o modelo escolha se consultará ou não o RAG;
- configure saída suficiente para a complexidade;
- rejeite truncamento;
- registre modelo, prompt version, tokens e finish reason.

### 4.2 RAG

Reestruture o RAG de forma incremental, separando:

- corpus jurídico oficial e versionado;
- legislação e regulamentação municipal;
- manuais institucionais;
- modelos aprovados como `STYLE_REFERENCE`;
- dados e documentos do processo atual.

Os fatos do processo atual devem ser fornecidos diretamente quando conhecidos. Modelos históricos não podem fornecer fatos.

Implemente metadados, filtros obrigatórios, busca híbrida, deduplicação e retrieval trace. O sistema deverá provar quais trechos foram usados.

Não presuma que a API acessa a internet automaticamente. Pesquisa web, quando necessária para atualização do corpus, deverá ser uma rotina separada, controlada e restrita a fontes oficiais.

### 4.3 Documento canônico

Implemente ou adapte uma representação JSON versionada conforme os schemas do pacote.

Ela deve conter:

- IDs estáveis;
- cláusulas e blocos;
- claims e source IDs;
- pendências;
- versão, parent version e hashes;
- metadados de geração;
- status.

DOCX e PDF devem ser gerados a partir dessa representação. Não use PDF como fonte de verdade.

### 4.4 Profundidade e perfis

Integre os perfis de DFD, ETP, TR e Mapa de Riscos como configuração versionada. Eles são guardrails, não metas cegas de palavras.

A geração deve considerar a complexidade do objeto e produzir cláusulas com densidade comparável aos documentos manuais, sem repetição artificial.

### 4.5 Auditoria consolidada

Implemente um auditor que receba o bundle completo, as fontes, os resultados determinísticos e os artefatos renderizados.

O auditor:

- não reescreve;
- retorna findings em schema;
- cruza DFD, ETP, riscos, TR e demais documentos;
- classifica gravidade;
- indica caminhos autorizados para correção;
- bloqueia findings altos e críticos;
- não promete certificação jurídica.

Execute inicialmente em shadow mode. Compare findings com avaliações conhecidas antes de ativar o gate.

### 4.6 Correção incremental

Implemente o Patch Service. O corretor recebe apenas findings, fontes, versões e caminhos autorizados.

Cada operação deve possuir:

- finding ID;
- documento;
- path;
- operação;
- hash do valor anterior;
- valor novo;
- fontes;
- justificativa.

Após aplicar, calcule diff. Rejeite qualquer alteração fora do escopo autorizado. Não regenere o documento completo.

### 4.7 Renderização

Preserve o template e a identidade visual já existentes quando corretos. Ajuste o motor para garantir:

- A4;
- Times New Roman 12 no corpo;
- espaçamento 1,5;
- 6 pt após parágrafo;
- texto justificado;
- títulos numerados e em negrito;
- tabelas dentro da área útil;
- cabeçalho, rodapé, logos e assinaturas preservados;
- ausência de cortes, sobreposição e páginas vazias indevidas.

Gere DOCX estruturado e converta para PDF. Implemente inspeção do DOCX, geometria do PDF e paridade textual.

## 5. Experiência do usuário

Adapte a interface para funcionar como assistente do servidor público:

- formulário condicional;
- extração e confirmação de informações;
- uma pergunta por etapa quando necessário;
- pendências claras;
- resumo antes da geração;
- status reais do workflow;
- painel de auditoria automatizada;
- diff das correções;
- emissão apenas após aprovação;
- mensagens acionáveis.

Não exponha prompts, tokens, vector stores ou detalhes internos ao usuário final.

## 6. Estratégia incremental obrigatória

Implemente na seguinte ordem, adaptando ao repositório:

1. instrumentação e prova do fluxo atual;
2. schemas e documento canônico;
3. Context Builder e retrieval trace;
4. gerador v2 por feature flag;
5. validações determinísticas;
6. renderer e preflight v2;
7. auditor em shadow mode;
8. patch incremental;
9. gate de auditoria;
10. expansão para todos os documentos.

Cada fase deve deixar o sistema executável.

## 7. Testes obrigatórios

Implemente testes unitários, integração e end-to-end para os casos do diretório `10_TESTES`.

Inclua obrigatoriamente:

- API ou chave ausente;
- RAG não consultado;
- fonte errada ou revogada;
- dado de outro processo;
- resposta truncada;
- schema inválido;
- divergência entre DFD, ETP e TR;
- cálculo incorreto;
- placeholder;
- fonte e espaçamento incorretos;
- texto fora da página;
- diferença DOCX/PDF;
- patch fora de escopo;
- isolamento entre tenants;
- caminho aprovado.

## 8. Git e entrega

Trabalhe em branch própria. Faça commits pequenos e descritivos. Não misture refatorações não relacionadas.

Ao finalizar, entregue:

1. diagnóstico do repositório;
2. matriz de compatibilidade do pacote;
3. arquitetura implementada;
4. lista de arquivos alterados;
5. migrations;
6. flags;
7. variáveis de ambiente;
8. testes executados e resultados;
9. evidência de chamada real à API;
10. evidência de retrieval;
11. exemplo de JSON canônico;
12. exemplo de auditoria;
13. exemplo de patch e diff;
14. DOCX e PDF antes/depois;
15. métricas de custo e latência;
16. instruções de deploy;
17. rollback;
18. limitações e riscos restantes.

Abra um pull request em modo draft, se o ambiente permitir, com resumo, checklist, riscos e plano de teste.

## 9. Critérios de aceitação

A tarefa só será considerada concluída quando:

- a implementação for incremental e compatível com o sistema atual;
- a API e o RAG estiverem comprovadamente ativos;
- cada documento usar no máximo uma chamada principal no caminho nominal;
- a saída for estruturada e versionada;
- o auditor revisar o conjunto;
- a correção preservar trechos não afetados;
- o renderer produzir documentos no padrão institucional;
- o sistema bloquear erros altos e críticos;
- o usuário for guiado com clareza;
- testes críticos passarem;
- houver rollback funcional.

Não responda apenas com uma proposta. Inspecione o repositório, apresente o diagnóstico e execute a implementação até o limite permitido pelo ambiente, sem apagar ou substituir de forma imprudente o que já funciona.
