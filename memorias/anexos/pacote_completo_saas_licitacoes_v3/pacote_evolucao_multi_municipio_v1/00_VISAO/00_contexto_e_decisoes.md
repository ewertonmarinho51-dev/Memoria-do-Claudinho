# Contexto e decisões consolidadas

## Contexto

O SaaS nasceu para apoiar servidores públicos na elaboração de documentos da fase preparatória das contratações. O sistema atual já possui funcionalidades, identidade visual, divisão por secretarias e um motor documental em evolução.

A plataforma deverá inicialmente atender um município com aproximadamente quinze secretarias e quantidade variável de usuários. Sua arquitetura, porém, deve ser preparada para a contratação futura por outros municípios sem exigir refatoração estrutural.

## Decisões consolidadas

1. A Lei nº 14.133/2021, regulamentos nacionais e manuais oficiais formam a base jurídica comum.
2. A principal variação entre municípios está nos modelos documentais, timbrados, cláusulas institucionais, fluxos, órgãos e pessoas.
3. A Lei Orgânica municipal não será o centro do modelo. Poderá existir no corpus quando necessária, mas não deverá dominar o desenho do produto.
4. Cláusulas estáveis, como obrigações, sanções e disposições institucionais aprovadas, deverão existir em catálogo versionado e bloqueado contra reescrita livre pela IA.
5. Cláusulas condicionais deverão ser aplicadas automaticamente por regras, conforme modalidade, objeto, sistema de registro de preços, tratamento favorecido e demais fatos estruturados.
6. O proprietário da plataforma deverá conseguir configurar município, secretarias, identidade visual, modelos e políticas.
7. O servidor público deverá enxergar uma interface simples, guiada e contextualizada.
8. Pessoas e assinaturas deverão ser escolhidas conforme vínculos e papéis institucionais, não por campos de texto livre.
9. O sistema deverá permitir identidade padrão do município e sobrescrita por secretaria.
10. O produto deverá continuar como monólito modular, quando essa for a arquitetura atual, até que carga real justifique decomposição.
