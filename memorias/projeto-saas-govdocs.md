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
- Skill caveman em `.claude/skills/caveman/` (economia de tokens).

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

- main = ef5e5d3 (app completo + Supabase + RAG + OpenAI + caveman).
- CI verde (última verificação: run #7; runs pós-OpenAI não checados
  ao vivo — mesmo código dos testes locais 19/19).
- Banco: processos OK; RAG estruturado e vazio (usuário ainda não
  indexou arquivos).

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
