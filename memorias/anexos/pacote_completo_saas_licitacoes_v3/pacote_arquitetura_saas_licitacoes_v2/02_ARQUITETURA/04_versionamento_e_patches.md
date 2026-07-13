# Versionamento e patches

## Identidade estável

Cada cláusula possui `clauseId` permanente, por exemplo `tr.execution.acceptance`. A numeração visual pode mudar, mas o ID não.

## Versão

Cada documento possui:

- `documentId`;
- `version` incremental;
- `parentVersion`;
- `contentHash`;
- `inputSnapshotHash`;
- `promptVersion`;
- `modelPolicyVersion`;
- `retrievalTraceId`.

## Patch protegido

Cada operação deve conter:

- alvo exato;
- operação permitida;
- hash anterior esperado;
- valor novo;
- finding que autorizou a alteração;
- fontes que sustentam a alteração;
- campos proibidos de alterar.

Se o hash não corresponder, o patch falha. Isso evita aplicar correção sobre uma versão diferente.

## Regra de preservação

O corretor deverá retornar a lista de nós alterados. Após a aplicação, o sistema calcula diff estrutural. Qualquer alteração fora dos caminhos autorizados reprova o patch.
