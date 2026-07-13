# 🧠 Memória do Projeto — GovDocs Wizard (projeto-saas)

> Arquivo de memória persistente entre sessões do Claude. Consultar no
> início de cada sessão de trabalho; atualizar a cada marco relevante.
> NUNCA gravar segredos aqui (chaves, tokens) — apontar onde estão.

Atualizado em: 2026-07-13

## O que é o projeto

App web (Python + Streamlit) que gera os 4 documentos da fase
preparatória de licitações (Lei 14.133/2021) em wizard sequencial:
Formulário Matriz → DFD (art. 12, VII) → ETP (art. 18) → TR (art. 40)
→ Minuta Edital/Ata SRP (art. 25). Preview editável + aprovação humana
por etapa; documento anterior aprovado vira contexto do próximo.
Exporta DOCX/PDF/ZIP (dossiê consolidado + individuais).

## Stack e arquitetura

- Streamlit (UI wizard) — módulos em `src/` (config, prompts, llm, db,
  rag, state, export, ui/)
- IA: OpenAI motor principal (padrão `gpt-5-mini`, via OPENAI_API_KEY),
  fallback automático Gemini (`gemini-2.5-flash`, GOOGLE_API_KEY).
  Retentativas backoff 2s/4s/8s, timeout 120s, erros amigáveis.
- Modo Demonstração offline (sem chave) p/ testes e CI.
- Supabase (projeto `nxibohgoekphxblqtqku`, "govdocs-wizard",
  sa-east-1, org xogoqqbzaxxnogqzuxef):
  - `processos`: autosave do wizard por etapa; painel lateral
    retomar/excluir. RLS anon liberado (tenant único, chave publishable).
  - RAG: `documentos_referencia` + `chunks_referencia` (pgvector 768 +
    tsvector português). Funções RPC `buscar_chunks_vetorial` /
    `buscar_chunks_textual` (operator(public.<=>) por causa de
    search_path=''). Migrações versionadas em `supabase/migrations/`
    (0003 aplicada manualmente pelo usuário no SQL Editor em 2026-07-07).
- RAG: página "📚 Base de Conhecimento" — upload PDF/DOCX/TXT/MD em 5
  categorias (lei, acórdão, entendimento, processo_anterior, modelo);
  chunks ~1500 chars; embeddings text-embedding-3-small dims=768
  (OpenAI) ou gemini-embedding-001 (fallback); top-6 injetado no prompt
  com regra "não copiar dados de outros processos". Trocar provedor de
  embeddings ⇒ reindexar.
- Testes: 19 (pytest + streamlit AppTest, modo demo, sem rede).
  `tests/conftest.py` insere raiz no sys.path (pytest binário não o faz).
- CI: GitHub Actions `.github/workflows/ci.yml` (python -m pytest).
- Skills do projeto em `.claude/skills/`: caveman (tokens), design-taste e design-minimalist (Front-End-Bonito; UI sempre com eles — setor público, azul #1B4F8A). Regras permanentes em CLAUDE.md do repo.

## Repos e branches

- Principal: `ewertonmarinho51-dev/projeto-saas` (público por decisão
  do usuário). Trunk: `main`. Branch de trabalho:
  `claude/procurement-docs-wizard-rhfl5o`.
- Memória: `ewertonmarinho51-dev/memoria-do-claudinho` (fork claude-mem;
  memórias em `memorias/`).
- Tokens/estilo: `ewertonmarinho51-dev/corta-tokens-oficial` (caveman).

## Segredos (ONDE estão, não O QUE são)

- `.streamlit/secrets.toml` local (gitignored): OPENAI_API_KEY,
  GOOGLE_API_KEY (opcional), SUPABASE_URL, SUPABASE_KEY (publishable).
- Token GitHub do usuário: fornecido em chat p/ push direto (relay da
  sessão 403). USUÁRIO DEVE REVOGAR ao fim do desenvolvimento; chave
  OpenAI idem (rotacionar) — avisado 2x.

## Decisões importantes (e porquês)

1. `google-genai` (SDK novo) — `google-generativeai` descontinuado.
2. OpenAI virou motor principal a pedido do usuário (2026-07-07).
3. RLS permissivo p/ anon: ferramenta interna tenant único; produção
   multiusuário ⇒ Supabase Auth + políticas auth.uid() (documentado).
4. Merges na main via git direto (API GitHub bloqueada pelo proxy do
   ambiente; PR #2 foi mergeado assim, marcado merged). Usuário
   autorizou push na main.
5. Commits ficam "Unverified" (sem assinatura — relay 403); cosmético.
6. Projetos Supabase antigos do usuário ("AGENTE ETP",
   qrgwuspkpznpagsnrfyy) pausados >90d, irrecuperáveis.

## Peculiaridades do ambiente remoto (Claude Code web)

- Egress bloqueia: *.supabase.co, api.openai.com, API GitHub direta.
  Git data-plane p/ github.com FUNCIONA com token do usuário.
- Origin do projeto-saas é resetado p/ relay local a cada restart do
  contêiner ⇒ push com URL tokenizada explícita.
- MCP Supabase caiu no meio da sessão e não voltou.
- Testes ao vivo de IA/banco impossíveis daqui ⇒ mocks + validação MCP
  (quando disponível) + validação final na máquina do usuário.

## Estado atual (2026-07-07)

- main = 74ab171: app + Supabase + RAG + OpenAI + redesign institucional
  (flat, sem emojis, #1B4F8A) + LOGIN/PAPÉIS (admin gerencia usuários,
  chaves de IA em config_app, identidade visual por órgão em
  config_orgaos com cabeçalho/rodapé PDF+DOCX e marca d'água PDF;
  usuário comum só wizard, processos filtrados por usuario_id; senhas
  PBKDF2 200k; bootstrap do 1º admin; modo aberto sem banco) + Dev
  Container do usuário. 29 testes. Migração 0004 PENDENTE de aplicação
  pelo usuário no SQL Editor.
- CI verde (última verificação: run #7; runs pós-OpenAI não checados
  ao vivo — mesmo código dos testes locais 19/19).
- Banco: processos OK; RAG estruturado e vazio (usuário ainda não
  indexou arquivos).

## Login-first + identidade visual por imagem (2026-07-07)

- LOGIN agora é porta de entrada real: sem Supabase conectado o app
  mostra tela "Configuração necessária" (não cai mais em modo aberto
  silencioso, que causava erro "supabase_url is required" ao criar
  usuário). Modo aberto virou opt-in: env GOVDOCS_MODO_ABERTO=1 (dev/CI).
  auth._tabela() e criar_usuario dão mensagem clara sem banco.
- IDENTIDADE VISUAL POR IMAGEM: novo src/branding.py. Admin envia
  documento-modelo (PDF, ou DOCX convertido via LibreOffice/soffice),
  PyMuPDF renderiza pág1, Pillow recorta cabeçalho (topo %), rodapé
  (base %) e marca d'água (miolo, alpha embutido no PNG). Imagens em
  base64 nas colunas config_orgaos.cabecalho_img/rodape_img/marca_img/
  cabecalho_pct/rodape_pct (migração 0005). export.py carimba: PDF
  header()/footer() desenha imagens em pos exata (proporção A4) + marca
  translúcida central; DOCX header/footer com add_picture. Fallback p/
  texto mantido. Admin UI: abas imagem/texto, upload+sliders+prévia.
- Deps novas: pymupdf, pillow (requirements); libreoffice (packages.txt
  p/ Streamlit Cloud). Testes: 36 (test_branding.py, test_auth.py ampliados).
- IMPORTANTE segurança: push protection do GitHub pegou .streamlit/
  secrets.toml.bak (backup temp que vazou no git add -A durante teste
  visual). Removido do commit antes de subir; .gitignore agora tem *.bak
  e secrets.toml.*. NÃO usar `mv secrets.toml *.bak` + git add -A.
- Migração 0005 PENDENTE de aplicação pelo usuário no SQL Editor.

## Padrão dos processos anteriores + memorando inicial (2026-07-08)

- Pedido: IA gerar no PADRÃO dos processos já feitos (estrutura, redação,
  cláusulas padrão/imutáveis), mas SEM copiar dados concretos de outro
  processo — dados só do processo atual. + campo pro memorando/ofício que
  inicia a demanda.
- FIX: (1) config.CAMPOS_FORMULARIO ganhou "memorando" (1º campo, area).
  steps: expander antes do form com file_uploader (pdf/docx/txt/md) que
  extrai via rag.extrair_texto → dados["memorando"] (jsonb, sem migração).
  prompts.montar_prompt injeta bloco próprio "MEMORANDO/OFÍCIO DO PROCESSO
  ATUAL"; formatar_dados_formulario PULA memorando (não duplica).
  (2) SYSTEM_PROMPT_BASE reforçado: "pegue como modelo e adapte ao novo
  objeto" (reaproveita só estrutura/linguagem/cláusulas), HIERARQUIA DE
  FONTES (1º lei/manuais, 2º processo atual, 3º padrão anteriores),
  compacidade permitida, faltou dado→[PREENCHER] nunca inventar/copiar.
  (3) rag.montar_bloco_referencias reformulado com o mesmo enquadramento.
- GOTCHA AppTest (de novo): 2 file_uploaders antes do st.form quebram a
  snapshot na transição form→etapa1 (KeyError $$ID-...). Solução: campos do
  form SEM key (senão gotcha key+value do Streamlit: upload/retomar não
  atualiza o campo) + test_app._iniciar_com_formulario passou a SEMEAR
  estado (dados+etapa=1) em vez de dirigir o form. Validação do envio fica
  em test_formulario_valida_campos_obrigatorios.
- Testes: 90 (novo tests/test_prompts.py). main=e96f1cb.

## gpt-5-mini resposta vazia -> troca de modelo (2026-07-08)

- Usuário: "OpenAI: resposta vazia do modelo" (chamada OK, mas gpt-5-mini
  devolveu content vazio — modelo de raciocínio gastou o orçamento de
  tokens pensando; finish_reason=length). O código só trocava de modelo em
  erro de modelo/404, então NÃO caía p/ gpt-4o-mini e ia direto ao Gemini.
  (O "tentados: gpt-5-mini, gpt-4o-mini,..." era só a lista de candidatos.)
- FIX: exceção llm._RespostaVazia + _trocar_de_modelo() = erro de modelo OU
  vazia → tenta próximo candidato (gpt-4o-mini/gpt-4o não são de raciocínio,
  respondem). Vale p/ OpenAI e Gemini. reasoning_effort gpt-5 "low"->"minimal"
  (sobra token p/ texto); série o mantém "low". detalhe agora lista modelos
  REALMENTE tentados. _traduzir_erro tem ramo p/ "vazia".
- Testes: 85. main=dd1845d.

## Limpeza de descrições de PDF (2026-07-08)

- Descrições da planilha do usuário vinham de PDF com espaços no meio de
  palavras ("plás tica","docu mentos","tungst ênio") e "?" no lugar de
  apóstrofo ("d?água"). planilha.limpar_texto conserta: ?→apóstrofo entre
  letras; espaço antes de pontuação; espaços duplos; e JUNTA palavra
  quebrada quando o 2º pedaço é fragmento de sufixo que nunca é palavra
  isolada (_FRAGMENTOS: tica,mentos,ado,ada,cao/coes(=ção),enio,dade,
  essidade,bilidade,tividade,avel/aveis,encia,ancia...). CONSERVADOR: não
  junta se o 2º pedaço é palavra real. CUIDADO: _core remove acento, então
  "são/cidade/idade/gráfica/ida/menta/do/da/ha" FICARAM DE FORA (colidem com
  palavra real). "cao" incluído (capta ção; risco de "cão" desprezível no
  domínio). Aplicada em _acrescentar (import) e calcular() — dados novos e
  já digitados; URLs (fonte) preservadas via eh_url. Casos "recicla do",
  "vermel ha", "a proximado" NÃO são corrigidos de propósito (2º pedaço =
  palavra real). Testes: 83. main=37a9679.

## Planilha grande: timeout do gpt-5 + qualidade (2026-07-08)

- SINTOMA: usuário com planilha de ~200 itens (materiais de expediente).
  Teste de conexão OK, mas gerar DFD: OpenAI "não respondeu/demorou" →
  fallback Gemini → doc com a lista redigitada (lento, com erros). Palavras
  quebradas ("plás tica") vêm dos DADOS do usuário (copiados de PDF), não
  são bug nosso.
- CAUSA: prompt embutia a tabela INTEIRA e pedia à IA reproduzir item a
  item. Com 200 itens + gpt-5-mini (reasoning), estourava
  tempo/max_completion_tokens (resposta vazia tratada como falha, 3 retries
  lentos → parecia timeout).
- FIX: tabelas > planilha.LIMITE_ITENS_INLINE(=12) → planilha.resumo_para_prompt
  (contagem + valor global + amostra de 6) e a IA insere a marca
  planilha.MARCADOR_TABELA("[[TABELA_ITENS]]"). planilha.injetar_tabela em
  gerar_documento troca a marca pela TABELA REAL (exata; anexa ao final se a
  IA esquecer). Tabelas pequenas seguem inline. para_markdown ganhou
  incluir_global=False (amostras).
- gpt-5/série o: _params_modelo_openai → reasoning_effort="low";
  max_completion_tokens 8192→16384; API_TIMEOUT_SEGUNDOS 120→180.
- Testes: 76. branch=8d204ec, main=a2ca854.

## CAUSA RAIZ da falha das duas APIs de IA (2026-07-08)

- BUG REAL (não era chave/modelo/cota): _obter_modelo_openai() e
  _obter_modelo() chamam _ler_chave("...MODEL", "") com o campo de sidebar
  VAZIO. _ler_chave fazia st.session_state.get("") → StreamlitAPIException
  ("key must be non-empty"). Isso estourava DENTRO de _chamar_openai/
  _chamar_gemini ao resolver o nome do modelo → AS DUAS engines falhavam
  SEMPRE. Só aparecia em runtime Streamlit (testes com _obter_modelo*
  monkeypatchados não pegavam). Detectado ao escrever llm.testar_conexao.
- FIX: _ler_chave só consulta a sessão se chave_sidebar for não-vazia.
- ROBUSTEZ: fallback automático de modelo em model_not_found/404
  (config OPENAI_MODELOS_FALLBACK=[gpt-4o-mini,gpt-4o,gpt-4.1-mini];
  GEMINI_MODELOS_FALLBACK=[gemini-1.5-flash,2.0-flash,flash-latest]).
  _e_erro_de_modelo distingue: erro de modelo troca de modelo; erro de
  chave/cota NÃO troca (falha igual em todos). _chamar_* iteram candidatos.
- DIAGNÓSTICO: llm.testar_conexao(motor)->(ok,msg) faz chamada mínima e
  devolve erro técnico exato; botões "Testar OpenAI/Gemini" no painel admin
  (aba Chaves de IA). detalhe do ErroGeracaoIA lista modelos tentados.
- Testes: 67. branch=fc8927b, main=8d0b924. LIÇÃO: bugs que só existem em
  runtime Streamlit (st.session_state, st.secrets) não são pegos por
  testes que monkeypatcham os getters — testar os getters DIRETO sem runtime.

## Import XLSX robusto + REBOBINADA do repo local (2026-07-08)

- BUG relatado: import de XLSX dava "Nenhum item reconhecido". Causa: o
  mapeamento de cabeçalho era só por IGUALDADE exata com sinônimos, então
  nomes reais por extenso ("Especificação do Objeto", "Descrição dos
  Serviços", "Preço Unitário (R$)") não casavam → toda linha sem descrição
  → descartada. Além disso, se um cabeçalho era detectado mas SEM a coluna
  descrição, não caía no fallback posicional (ficava vazio).
- FIX planilha.importar_de_xlsx: novo _campo_do_cabecalho em 3 níveis
  (igualdade > palavra inteira > raiz por substring, via _RAIZES). Só
  aceita cabeçalho que reconheça a DESCRIÇÃO + 1 coluna; senão fallback
  posicional. Varre TODAS as abas (1ª pode ser capa) e pula linhas de
  título. Coluna "Total"/"Valor Total" do arquivo é ignorada (recalculada).
  Erro mostra o cabeçalho lido. Testes: 60 (+4). branch=2e869c9, main=4ab2b13.
- ⚠️ GOTCHA GRAVE DO AMBIENTE: entre turnos o repo local FOI REBOBINADO —
  git HEAD voltou p/ base antiga (76ea011/18558cc) SEM o commit já pushado
  (41a1b69, fix das APIs), embora os ARQUIVOS em disco ainda tivessem as
  edições. Um commit "por cima" gerou histórico divergente e push rejeitado
  (non-fast-forward). SOLUÇÃO que funcionou: tratar github como verdade —
  `git fetch <URL> main <branch>`, recriar branch a partir do SHA REAL do
  github (git checkout -B branch 41a1b69), `git cherry-pick <meu_commit>`,
  push ff; refazer main = checkout -B main <sha_github> + merge + push.
  SEMPRE conferir `git ls-remote <URL>` (github real) — o remote "origin"
  aponta p/ relay 127.0.0.1 e pode divergir. NUNCA force-push por cima.

## Planilha orçamentária + import XLSX (2026-07-08)

- Campo único "valor estimado" virou PLANILHA de itens (código,
  descrição, unidade, quantidade, valor unitário) via st.data_editor
  dinâmico. valor_total por item e VALOR GLOBAL (soma) calculados; o
  global alimenta dados["valor_estimado"] (fluxo a jusante intacto).
  Módulo src/planilha.py (calcular, formatar_moeda BR, para_markdown).
  A planilha entra no prompt como tabela Markdown → IA reproduz na
  estimativa de valor. AppTest não dirige data_editor: testes semeiam
  session_state["dados"]["itens"].
- IMPORT XLSX: planilha.importar_de_xlsx (openpyxl) detecta cabeçalho por
  sinônimos sem acento (SINONIMOS), fallback posicional, _num aceita
  moeda BR "1.234,56". Expander no form: baixar modelo_xlsx() + upload
  que re-semeia a tabela (key do data_editor = _xlsx_lido para forçar
  reload). openpyxl no requirements.
- Testes: 46. main = 50e2353. Token do usuário voltou a ter escrita
  (403 anterior era transitório, não rotação).

## Coluna Fonte/Link e links clicáveis (2026-07-08)

- Planilha ganhou coluna opcional 'fonte' (Fonte/Link, LinkColumn com
  display_text='link') + preserva colunas extras vindas do XLSX
  (importar_de_xlsx mantém colunas não mapeadas; SINONIMOS inclui fonte/
  link/url). calcular() e para_markdown() preservam extras. URLs viram
  [link](url) no markdown (planilha.para_link_markdown, normaliza www->https).
- EXPORT: links [texto](url) viram hyperlink clicável real — DOCX via
  w:hyperlink XML (_docx_hyperlink, _docx_runs_ricos, _segmentos_ricos),
  PDF via fpdf2 markdown=True. Tabelas Markdown no PDF agora são TABELAS
  REAIS (pdf.table, _pdf_render_tabela) com quebra de texto — corrige
  estouro de margem com muitas colunas. Prompt instrui preservar links.
- APPTEST GOTCHA IMPORTANTE: st.data_editor + widgets keyless antes do
  st.form quebram a snapshot do AppTest (KeyError $$ID-...-None em
  TextInput/Selectbox.value; acúmulo de estado entre reruns). Por isso o
  expander manual 'Adicionar coluna' foi REMOVIDO (colunas extras vêm do
  XLSX). Testes que forçam session_state['etapa'] devem SEMEAR o estado
  ANTES do 1o at.run() (não via _iniciar), evitando render do data_editor
  (ver test_sequencia/test_edicao reescritos). 52 testes. main=76ea011.

## Diagnóstico de falha das APIs (2026-07-08)

- Usuário relatou "as duas APIs (GPT e Google) com problema". Sem acesso
  vivo daqui (egress bloqueia api.openai.com). Causa não era o código de
  seleção de motor — era a MENSAGEM DE ERRO genérica escondendo o motivo
  real (401/404/429/região).
- FIX: _traduzir_erro(exc, motor) agora é por engine — aponta chave certa
  (OPENAI_API_KEY vs GOOGLE_API_KEY), modelo (OPENAI_MODEL vs GEMINI_MODEL)
  e painel de cada provedor; cobre insufficient_quota, invalid_api_key,
  model_not_found, billing, rate limit, unsupported. ErroGeracaoIA carrega
  .detalhe (erro bruto = motor + modelo + tipo + msg). UI: expander
  "Detalhes técnicos" na tela de geração + no aviso de fallback.
- LEMBRAR: _ler_chave lê db.obter_config (painel admin/config_app) ANTES
  de secrets/env. Chave errada salva no painel do admin sobrepõe a correta
  do secrets — suspeito nº1 quando "as duas falham juntas".
- 56 testes (+4). main = 4583c4a. AGUARDANDO: usuário rodar geração e
  colar o "Detalhes técnicos" p/ confirmar causa (chave? modelo? cota?).

## Pendências / próximos passos

- [ ] Usuário testar geração real com chave OpenAI na máquina dele
      (validar modelo gpt-5-mini disponível na conta).
- [ ] Popular Base de Conhecimento (Lei 14.133 PDF, acórdãos TCU,
      orientações TCE, ETPs/TRs antigos).
- [ ] Deploy (Streamlit Community Cloud) — usuário ainda não pediu.
- [ ] Fim do desenvolvimento: revogar token GitHub + rotacionar chave
      OpenAI; considerar repo privado de novo (decisão dele: público).
- [ ] Futuro possível: Supabase Auth multiusuário; instalar claude-mem
      completo na máquina local do usuário (npx claude-mem install).

## Origem das chaves + push via patch (2026-07-11)

- llm.origem_chave + indicador na aba Chaves de IA (prioridade: painel >
  sidebar > secrets > env). Chaves novas do usuário postas em
  .streamlit/secrets.toml local (gitignored). Chave "Gemini" fornecida
  (AQ.Ab8...) tem formato suspeito (não é AIzaSy... do AI Studio) — avisado.
- SESSÃO PERDEU ESCRITA NO GITHUB (403 em token do usuário, relay e app;
  2 tokens novos também falharam — bloqueio do ambiente, não do token).
  Solução: git format-patch enviado ao usuário, aplicado na máquina dele
  com git am + push próprio. main=2e13e55 (92 testes ok, verificado).
- Assinatura de commits: /tmp/code-sign só foi instalado 2026-07-11 15:12;
  commits anteriores ficam Unverified (cosmético, decisão registrada).
- Tokens ghp_* expostos em chat: usuário orientado a revogar TODOS e
  rotacionar chaves OpenAI/Gemini ao fim do desenvolvimento.

## Motor de geração no padrão institucional (2026-07-12)

- Usuário enviou ZIPs: docs GERADOS vs docs MANUAIS aprovados (Paragominas).
  Medição: manuais Times 12, DFD 4.804/ETP 12.541/TR 11.412 palavras,
  9/18/17 cláusulas, numeração 1.1.1.; gerados Helvetica 11, 5x menores,
  texto FORA da margem direita, 6-28 [PREENCHER] no final.
- CAUSAS: prompt com estrutura própria (≠ padrão da casa) + "enxuto" +
  reasoning minimal; export fpdf2 desenhando direto (Helvetica, sem estilos).
- FIX (commit 1f96200, local — push ainda 403, patch enviado ao usuário):
  perfis.py (estrutura/metas por cláusula extraídas dos manuais, config
  central); prompts via perfis + proibição de mencionar mecânica interna;
  export.py DOCX com estilos GovDocs (Times 12/1,5/6pt/justif/keep_next/
  tblHeader/cantSplit; run.bold só quando True senão anula estilo!) e PDF
  via DOCX->LibreOffice (fallback fpdf2 Times; motor_pdf()); validacao.py
  (PREENCHER/[[TABELA_ITENS]]/menções internas BLOQUEIAM download; avisos
  numeração/raso/cláusula ausente); llm.registrar_geracao (tokens/req-id/
  duração/fallback, log+sessão); Gemini 16384; reasoning low.
- GOTCHA ambiente: container só tinha libreoffice-core (sem writer →
  "source file could not be loaded"); apt install libreoffice-writer
  resolveu. Streamlit Cloud ok (packages.txt=libreoffice completo).
- 111 testes. Pendências/backlog: edital sem manual de referência; Mapa
  de Riscos como doc próprio; regeneração por cláusula (JSON estruturado).

## Escrita no GitHub restaurada + crash PDF + Fase 1 multi-tenant (2026-07-12)

- Token novo do usuário (4º) VOLTOU a ter escrita; pushes normalizados.
  Usuário aplicou motor-geracao.patch v1 na máquina dele (=311da6b);
  cherry-pick do fix por cima. main=eea3613; branch sincronizado.
- CRASH produção: fpdf2 "row too high" (linha de tabela > página; fpdf2
  não divide linha). Fix: _pdf_render_tabela tenta fontes 9/7/6pt e
  degrada p/ parágrafos "Rótulo: valor" — download nunca quebra. (Rota
  principal já é DOCX→LibreOffice.) GOTCHA descoberto antes: container
  local só tinha libreoffice-core; produção Cloud ok (packages.txt).
- PACOTES v2/multi-município: auditoria entregue em
  docs/matriz-compatibilidade.md (classificação por capacidade, fases
  1-5, decisões pendentes: Supabase Auth na Fase 2; catálogo de cláusulas
  só com textos aprovados do usuário; edital segue sem modelo manual).
- FASE 1 IMPLEMENTADA: migração 0006 (tenants c/ Paragominas uuid fixo
  11111111-...-1111 como DEFAULT de tenant_id em processos/usuarios/
  config_orgaos/config_app/documentos_referencia; processos.snapshot;
  tabela geracoes) — PENDENTE usuário aplicar no SQL Editor.
  db.tenant_atual()/registrar_geracao_bd (best-effort) +
  llm.registrar_geracao persiste. 116 testes.
- Lembrete de segurança PENDENTE: revogar os 4 tokens ghp_ expostos no
  chat + rotacionar chaves OpenAI/Gemini ao fim.

## Pacote v3 completo recebido e arquivado (2026-07-13)

- Usuário enviou MASTER_PROMPT_IMPLEMENTACAO.md + pacote_completo_saas_
  licitacoes_v3.zip = pacote_arquitetura_saas_licitacoes_v2 + pacote_
  evolucao_multi_municipio_v1, agora com os 153 arquivos completos
  (a auditoria de 2026-07-12 foi feita só com resumos). Master prompt
  avulso é idêntico ao de dentro do pacote evolucao (sha c1acd1bf...).
  ARQUIVADO em memorias/anexos/pacote_completo_saas_licitacoes_v3/
  neste repo (uploads e scratchpad da sessão são efêmeros). Sem segredos
  no conteúdo (verificado).
- Pacote v2 (motor): diagnóstico gerado-vs-manual (+ montagens jpg),
  arquitetura alvo (pipeline determinístico, orçamento de chamadas,
  máquina de estados, versionamento/patches, observabilidade), RAG
  (ingestão/retrieval/avaliação/prompt-injection), 11 prompts prontos,
  8 schemas JSON canônicos (document-bundle, generated-document,
  process-context, audit-report, patch-plan...), perfis documentais
  JSON (dfd/etp/tr/mapa-riscos/formatting), validações, UX,
  implementação em fases 0-8, testes (test-cases.json), configs
  (feature-flags, model-policy, rag-config, review-config).
- Pacote evolucao v1 (multi-município): hierarquia PLATAFORMA→MUNICÍPIO
  →SECRETARIA→PESSOAS→PROCESSOS→DOCUMENTOS; herança document_snapshot >
  document_type_override > secretariat > tenant > platform_default;
  identidade visual herdável c/ override por secretaria; modelos
  versionados por blocos (FIXED_LOCKED, AI_GENERATED, CONDITIONAL_LOCKED,
  DATA_BOUND, COMPUTED, SIGNATURE_SLOT, TABLE_SLOT); catálogo de
  cláusulas versionado (nunca inventar conteúdo jurídico — migrar textos
  aprovados); motor de políticas declarativo (DSL + simulador + conflito
  bloqueia, nunca escolhe em silêncio); pessoas/vínculos/papéis/slots de
  assinatura por ELEGIBILIDADE (secretário nunca aparece como elaborador;
  vínculo vencido some; snapshot congela nome/cargo); painéis proprietário
  vs servidor; 9 schemas de dados; fases 0-7. Alvo inicial: 1 município
  (Paragominas) com ~15 secretarias.
- Feature flags previstas: MULTI_TENANT_CONTEXT_V1, SECRETARIAT_CONTEXT_V1,
  BRANDING_RESOLVER_V1, VERSIONED_TEMPLATES_V1, INSTITUTIONAL_PEOPLE_V1,
  SIGNATURE_ELIGIBILITY_V1, CLAUSE_CATALOG_V1, POLICY_ENGINE_SHADOW_V1/
  ENFORCE_V1, INSTITUTIONAL_CONTEXT_SNAPSHOT_V1 (todas default false,
  escopo tenant).
- MAPEAMENTO vs. já implementado: auditoria/matriz = docs/matriz-
  compatibilidade.md (2026-07-12); Fase 0 parcial (116 testes,
  registrar_geracao, sem flags formais); Fase 1 tenant padrão FEITA
  (migração 0006 — aplicação no SQL Editor ainda PENDENTE). perfis.py/
  validacao.py/export DOCX→LibreOffice cobrem parte das fases 0/4 do
  pacote motor (ainda sem JSON canônico; geração segue markdown).
- PRÓXIMO PASSO: Fase 2 — secretarias + contexto derivado da sessão
  (normalizar secretarias, vincular usuários, resolver contexto
  automaticamente). Decisão pendente ligada: Supabase Auth vs. auth
  própria atual (PBKDF2). Implementação acontece no repo projeto-saas.
