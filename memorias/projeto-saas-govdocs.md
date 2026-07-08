# 🧠 Memória do Projeto — GovDocs Wizard (projeto-saas)

> Arquivo de memória persistente entre sessões do Claude. Consultar no
> início de cada sessão de trabalho; atualizar a cada marco relevante.
> NUNCA gravar segredos aqui (chaves, tokens) — apontar onde estão.

Atualizado em: 2026-07-07

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
