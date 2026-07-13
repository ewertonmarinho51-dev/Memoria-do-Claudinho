# Resumo executivo do diagnóstico

A diferença entre os documentos manuais e os gerados não é apenas estética. Ela envolve perda de conteúdo, ausência de recuperação confiável, falta de controle de saída e erro no motor de composição.

## Sinais principais

- os documentos manuais usam predominantemente Times New Roman, tamanho 12;
- os documentos gerados usam predominantemente Helvetica, tamanho 11;
- DFD, ETP e TR gerados têm apenas uma fração do volume dos manuais;
- existem elementos posicionados fora da área da página;
- o sistema aparenta misturar geração semântica com montagem visual;
- placeholders e textos de orientação podem chegar à saída final;
- não há evidência suficiente de que o RAG foi consultado;
- uma simples regeneração pode causar deriva em todo o documento.

## Conclusão

A correção deve atuar em quatro camadas:

1. recuperação e montagem do contexto;
2. geração estruturada com contrato rígido;
3. auditoria e patch incremental;
4. renderização determinística e inspeção visual.
