# PROMPT MESTRE — EVOLUÇÃO MULTI-MUNICÍPIO, POLÍTICAS DOCUMENTAIS E ASSINATURAS

Você recebeu dois pacotes complementares:

1. `pacote_arquitetura_saas_licitacoes_v2`, que define o motor de geração, RAG, auditoria, correções incrementais, schemas canônicos, validação e renderização;
2. `pacote_evolucao_multi_municipio_v1`, que define multi-tenancy, secretarias, identidade visual herdável, modelos documentais, catálogo de cláusulas, motor de políticas, pessoas, papéis e assinaturas.

O repositório já está funcionando. Sua tarefa é analisar o código real, verificar compatibilidade e implementar incrementalmente as propostas úteis. Não reescreva o projeto do zero, não imponha uma stack diferente e não remova funcionalidades ativas sem migração, testes, feature flag e rollback.

## 1. Primeiro resultado obrigatório: auditoria e matriz de compatibilidade

Antes de alterar código, inspecione:

- arquitetura e stack;
- autenticação, autorização e sessão;
- modelos de usuário, órgão, secretaria, município e processo;
- banco, ORM e migrations;
- frontend e fluxos atuais;
- motor de geração, RAG, auditoria e renderização;
- templates, timbrados e identidade visual;
- assinaturas e responsáveis;
- armazenamento;
- filas e workers;
- logs, métricas, CI e deploy;
- qualquer suporte multi-tenant já existente.

Produza uma matriz:

`capacidade proposta | implementação atual | lacuna | compatibilidade | reutilizar/adaptar/criar | arquivos | migration | risco | testes | decisão`

Classifique cada item dos dois pacotes como:

- já implementado;
- compatível e reutilizável;
- compatível com adaptação;
- incompatível, mas com objetivo aproveitável;
- desnecessário no momento;
- fase posterior.

Não comece uma reescrita antes dessa auditoria.

## 2. Objetivo de produto

O sistema deverá funcionar com esta separação:

- o proprietário da plataforma configura municípios, secretarias, modelos, identidades, cláusulas, políticas, pessoas e permissões;
- o servidor público informa os fatos da contratação e confirma responsáveis;
- o sistema resolve automaticamente o contexto institucional;
- o motor documental gera apenas o conteúdo variável;
- cláusulas aprovadas e imutáveis são inseridas deterministicamente;
- assinaturas são filtradas por vínculo e papel;
- nenhum detalhe técnico de IA é exposto ao servidor.

A experiência do usuário final deve ser simples. Ele não deve escolher timbrado, versão de cláusula, política, template interno, prompt ou fonte jurídica.

## 3. Fundamentos multi-tenant sem ruptura

Implemente fundações para múltiplos municípios, preservando o município atual como tenant padrão.

Requisitos:

- `tenant_id` obrigatório em entidades municipais;
- `secretariat_id` onde houver escopo de secretaria;
- contexto derivado da autenticação e dos vínculos, nunca confiado a um campo livre do frontend;
- isolamento em banco, storage, cache, filas, logs, RAG e arquivos;
- testes negativos de acesso cruzado;
- URLs de arquivo autorizadas e temporárias;
- jobs com tenant explícito;
- snapshots de contexto.

Use banco compartilhado com isolamento lógico, salvo se a arquitetura atual ou requisito real justificar outra estratégia. Não crie microserviços apenas para declarar escalabilidade.

## 4. Hierarquia institucional

Implemente ou adapte:

`PLATAFORMA -> MUNICÍPIO -> SECRETARIA -> USUÁRIOS/PESSOAS -> PROCESSOS -> DOCUMENTOS`

Configurações devem seguir herança:

`document_snapshot > document_type_override > secretariat > tenant > platform_default`

O proprietário da plataforma define quais campos podem ser sobrescritos. Toda resolução deve indicar a origem do valor e gerar hash/versionamento.

## 5. Identidade visual e modelos

O município possui identidade padrão. Cada secretaria pode herdar ou sobrescrever timbrado, logo, cabeçalho, rodapé, endereço, contatos e demais elementos permitidos.

Quando o servidor gera um documento, município e secretaria são resolvidos pelo processo e vínculo. O timbrado correto deve ser aplicado automaticamente.

Transforme o conceito de modelo em estrutura versionada, com blocos:

- `FIXED_LOCKED`;
- `AI_GENERATED`;
- `CONDITIONAL_LOCKED`;
- `DATA_BOUND`;
- `COMPUTED`;
- `SIGNATURE_SLOT`;
- `TABLE_SLOT`.

Versões publicadas são imutáveis. Alteração cria nova versão. Documentos emitidos preservam a versão usada.

## 6. Catálogo de cláusulas

Implemente um catálogo versionado para textos estáveis, incluindo famílias como obrigações, gestão, fiscalização, sanções, recebimento, pagamento e disposições institucionais.

Cada cláusula deve possuir:

- código estável;
- versão;
- escopo;
- tipos documentais compatíveis;
- conteúdo aprovado;
- fonte e justificativa;
- vigência;
- variáveis permitidas;
- hash;
- status de aprovação;
- bloqueio contra reescrita pela IA.

Uma versão publicada não é editada. Crie nova versão para qualquer mudança.

Não invente o conteúdo jurídico das cláusulas. Migre textos já aprovados do sistema ou dos documentos institucionais para estado de revisão e exija publicação controlada.

## 7. Motor de políticas documentais

Implemente motor declarativo para aplicar regras condicionais com base em fatos estruturados, como tipo documental, modalidade, categoria do objeto, registro de preços, regime, valor, risco e demais campos reais do sistema.

As políticas podem:

- incluir ou excluir cláusula;
- exigir campo;
- adicionar validação;
- selecionar template;
- configurar slot de assinatura.

Use DSL limitada e validada. Não execute código cadastrado e não espalhe regras em condicionais arbitrárias no código.

Inclua simulador administrativo que mostre entradas, regras ativadas, ações, conflitos e versões.

Em caso de conflito não resolvido por prioridade e especificidade, bloqueie a publicação ou geração. Não escolha silenciosamente.

## 8. Composição do documento

Conecte esta camada ao motor do pacote anterior:

1. resolver tenant e secretaria;
2. resolver branding e template;
3. avaliar políticas;
4. selecionar cláusulas fixas e condicionais;
5. identificar slots variáveis;
6. montar contexto do processo e RAG;
7. realizar uma chamada principal por documento para os slots geráveis;
8. validar o JSON canônico;
9. compor todos os blocos deterministicamente;
10. calcular numeração e referências;
11. renderizar;
12. auditar e corrigir por patches.

A IA nunca deverá receber liberdade para reescrever cláusulas `FIXED_LOCKED` ou `CONDITIONAL_LOCKED`.

## 9. Pessoas, vínculos e papéis

Não trate assinaturas como campos de texto livre.

Implemente ou adapte:

- pessoa institucional;
- vínculo com município e secretaria;
- cargo;
- papéis documentais;
- vigência;
- titularidade ou substituição;
- ato de designação quando aplicável;
- estado.

Papéis iniciais podem incluir elaborador, equipe de planejamento, secretário, autoridade de concordância, aprovador, fiscal e gestor.

O sistema deverá listar pessoas por elegibilidade, não apenas por nome.

## 10. Slots de assinatura

Cada template define slots com papéis elegíveis, quantidade, ordem, escopo e obrigatoriedade.

Regra padrão solicitada:

- pessoa com papel exclusivo de secretário não aparece como elaborador;
- secretário ativo aparece nos slots de concordância ou aprovação previstos;
- elaboradores são servidores ou membros elegíveis da secretaria;
- exceções somente pelo proprietário, versionadas e auditadas.

A elegibilidade considera a data do documento. Pessoas com vínculo encerrado não aparecem para documentos posteriores.

O snapshot do documento congela nome, cargo, secretaria, papel e vínculo usados. Mudanças futuras não alteram documentos emitidos.

## 11. Painéis

### Proprietário da plataforma

Implementar módulos para:

- municípios;
- secretarias;
- identidade visual;
- templates;
- cláusulas;
- políticas e simulador;
- pessoas e vínculos;
- papéis e permissões;
- feature flags;
- trilha de auditoria;
- saúde da plataforma.

### Servidor público

Manter jornada enxuta:

- processos da secretaria;
- perguntas guiadas;
- pendências;
- pessoas elegíveis;
- status da geração e auditoria;
- emissão.

Não exponha configurações técnicas ou jurídicas internas.

## 12. Migração incremental

Execute preferencialmente:

1. testes e instrumentação;
2. tenant padrão e backfill;
3. secretarias e contexto da sessão;
4. branding resolver em shadow mode;
5. templates versionados;
6. pessoas e vínculos;
7. assinatura por elegibilidade;
8. catálogo de cláusulas;
9. motor de políticas em shadow mode;
10. enforcement por tenant;
11. integração completa ao motor documental;
12. segundo tenant piloto.

Use migrations expand/contract. Não exclua estruturas legadas na mesma release. Mantenha adaptadores e rollback.

## 13. Hospedagem e operação

Para a escala inicial, prefira monólito modular, aplicação stateless, banco relacional gerenciado, object storage, fila e workers para geração e renderização.

Implemente:

- idempotência;
- retry controlado;
- limites de concorrência;
- observabilidade por etapa e tenant;
- backups e teste de restauração;
- alertas de fila, erro, custo e isolamento;
- correlação de logs.

Não decomponha em microserviços sem necessidade demonstrada.

## 14. Manutenção assistida por IA

Prepare integração segura para que uma IA analise logs, reproduza falhas, crie branch, implemente testes e abra pull request.

Ela não deverá alterar produção diretamente. O fluxo inicial deve exigir CI, revisão, canário e rollback.

## 15. Testes obrigatórios

Implemente os casos do diretório `10_TESTES`, incluindo:

- isolamento entre tenants;
- contexto derivado da sessão;
- herança visual;
- sobrescrita da secretaria;
- imutabilidade de versões;
- cláusula fixa preservada;
- política aplicada e não aplicada;
- conflito de políticas;
- secretário excluído do slot de elaborador;
- servidor elegível;
- vínculo vencido;
- substituto vigente;
- snapshot preservado;
- RAG isolado;
- cache isolado;
- rollback;
- segundo tenant piloto.

Mantenha testes de regressão do fluxo atual com flags desativadas.

## 16. Entregas obrigatórias

Ao concluir cada fase, entregue:

1. diagnóstico do repositório;
2. matriz de compatibilidade;
3. desenho adaptado à stack real;
4. arquivos alterados;
5. migrations e backfill;
6. feature flags;
7. testes e resultados;
8. evidências de isolamento;
9. screenshots ou previews dos painéis;
10. exemplo de contexto resolvido;
11. exemplo de política simulada;
12. exemplo de cláusula versionada;
13. exemplo de elegibilidade de assinatura;
14. documento com branding municipal;
15. documento com override de secretaria;
16. procedimento de deploy;
17. procedimento de rollback;
18. riscos remanescentes.

Não entregue apenas sugestões ou pseudocódigo. Implemente até o limite permitido pelo ambiente, com commits pequenos, rastreáveis e compatíveis com o sistema atual.
