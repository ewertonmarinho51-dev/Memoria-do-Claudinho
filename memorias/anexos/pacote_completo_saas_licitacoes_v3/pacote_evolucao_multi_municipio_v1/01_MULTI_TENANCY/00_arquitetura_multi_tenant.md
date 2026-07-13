# Arquitetura multi-tenant incremental

## Hierarquia

```text
PLATAFORMA
└── MUNICÍPIO (tenant)
    ├── SECRETARIA / UNIDADE ORGANIZACIONAL
    │   ├── USUÁRIOS
    │   ├── PESSOAS INSTITUCIONAIS
    │   ├── CONFIGURAÇÕES VISUAIS
    │   └── PROCESSOS E DOCUMENTOS
    ├── MODELOS DOCUMENTAIS
    ├── CATÁLOGO DE CLÁUSULAS
    ├── POLÍTICAS DOCUMENTAIS
    └── CORPUS RAG INSTITUCIONAL
```

## Estratégia inicial recomendada

Para a escala prevista, priorizar banco compartilhado com `tenant_id` obrigatório, controle de acesso em todas as consultas e isolamento lógico rigoroso. Banco por tenant somente deverá ser adotado caso exigência contratual, regulatória ou de escala justifique.

## Contexto obrigatório da requisição

Toda operação autenticada deverá resolver:

- `actor_user_id`;
- `tenant_id`;
- `secretariat_id`, quando aplicável;
- papéis e permissões;
- configuração institucional ativa;
- versão das políticas.

O backend não deverá confiar em `tenant_id` enviado livremente pelo frontend. Ele deve ser derivado da sessão, token ou associação autorizada.

## Regra de ouro

Toda entidade pertencente a um município deverá possuir `tenant_id`, inclusive configurações, pessoas, arquivos, vetores, prompts customizados, logs de domínio, documentos e versões.
