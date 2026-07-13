# Arquitetura alvo

```mermaid
flowchart LR
    U[Usuário] --> I[Intake guiado]
    I --> V[Validação determinística]
    V --> C[Context Builder]
    C --> R[(RAG institucional e jurídico)]
    C --> G[Gerador com Structured Output]
    G --> J[(JSON canônico versionado)]
    J --> D[Validações determinísticas]
    D --> X[Renderer DOCX]
    X --> P[Conversor PDF]
    P --> Q[Preflight visual e geométrico]
    Q --> A[Auditor consolidado]
    A -->|aprovado| E[Emissão final]
    A -->|reprovado| F[Plano de patch]
    F --> K[Corretor incremental]
    K --> J
```

## Componentes

1. **Intake Service:** coleta dados e gera pendências.
2. **Source Registry:** registra fontes, versões, validade e escopo.
3. **RAG Service:** busca híbrida com filtros.
4. **Context Builder:** seleciona e compacta o contexto sem LLM.
5. **Generation Service:** uma chamada por documento e saída estruturada.
6. **Canonical Document Store:** versionamento e hashes.
7. **Rule Engine:** validações objetivas.
8. **Renderer:** DOCX a partir de template institucional.
9. **PDF Pipeline:** conversão e inspeção.
10. **Audit Service:** auditoria semântica do bundle.
11. **Patch Service:** aplicação localizada e protegida.
12. **Workflow Engine:** estados, retries, bloqueios e feature flags.
13. **Observability:** logs, métricas, traces e custos.
