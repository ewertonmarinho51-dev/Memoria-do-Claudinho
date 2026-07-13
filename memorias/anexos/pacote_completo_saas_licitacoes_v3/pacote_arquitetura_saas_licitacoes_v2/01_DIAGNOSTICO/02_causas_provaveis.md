# Causas prováveis a confirmar no repositório

Estas hipóteses não devem ser tratadas como conclusão sem inspeção do código e dos logs.

1. Chave de API disponível, mas rota de geração usa mock ou fallback.
2. Erro da API capturado e substituído por template genérico.
3. Limite de saída pequeno ou truncamento da resposta.
4. Prompt envia apenas resumo do formulário.
5. RAG indexado, mas não chamado pela rota ativa.
6. Retrieval sem filtros retorna trechos irrelevantes.
7. Modelo recebe documentos inteiros sem seleção e perde foco.
8. Saída livre é parseada por expressões frágeis.
9. Conteúdo é desenhado diretamente em PDF com coordenadas fixas.
10. Conversão DOCX-PDF ocorre em ambiente sem as fontes corretas.
11. Regeneração substitui a versão inteira, sem IDs estáveis.
12. Não existe auditoria de coerência entre DFD, ETP e TR.

## Evidências mínimas exigidas

A implementação deverá produzir logs com:

- rota acionada;
- modelo;
- prompt version;
- quantidade de contexto;
- IDs dos trechos recuperados;
- tokens de entrada e saída;
- motivo de finalização;
- schema validado ou rejeitado;
- fallback utilizado ou não;
- hash do documento canônico;
- resultado da renderização.
