# Plano incremental

## Fase 0: diagnóstico

Sem alterar comportamento, instrumentar geração atual e provar API, RAG e renderer.

## Fase 1: documento canônico

Introduzir schemas, storage de versões e adapter que converte a saída atual.

## Fase 2: Context Builder e retrieval trace

Substituir recuperação implícita por consultas declarativas e filtros.

## Fase 3: novo gerador por feature flag

Ativar inicialmente para um tipo documental e um tenant de teste.

## Fase 4: renderer determinístico

Aplicar template DOCX e preflight PDF.

## Fase 5: auditor bundle

Executar em modo sombra, comparar resultados e calibrar.

## Fase 6: patch incremental

Ativar correção localizada com diff protegido.

## Fase 7: gate de emissão

Somente após taxa de falsos bloqueios aceitável.

## Fase 8: expansão

ETP, TR, riscos e demais documentos.
