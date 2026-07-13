# Evolução Multi-Município, Políticas Documentais e Assinaturas Institucionais

Versão: 1.0  
Data: 12/07/2026  
Natureza: pacote complementar ao `pacote_arquitetura_saas_licitacoes_v2`

## Finalidade

Este pacote transforma o SaaS existente em uma plataforma preparada para atender múltiplos municípios, preservando o foco atual na geração documental. A proposta não exige reescrita nem adoção imediata de microserviços. Ela introduz fundações incrementais para:

- isolamento por município;
- estrutura por secretarias;
- identidade visual herdável;
- modelos documentais por município;
- catálogo versionado de cláusulas imutáveis;
- motor de políticas documentais condicionais;
- cadastro institucional de pessoas, cargos e funções;
- assinaturas compatíveis com o papel de cada pessoa;
- experiência simplificada para o servidor público;
- operação escalável e manutenção assistida por IA com controles.

## Princípio de produto

> O administrador configura a instituição. O servidor informa apenas os fatos da contratação. O sistema resolve automaticamente regras, modelos, cláusulas, identidade visual, pessoas elegíveis e assinaturas.

## Relação com o pacote anterior

O pacote anterior define o motor de geração, RAG, auditoria, correções por patch, schemas canônicos e renderização. Este pacote acrescenta a camada organizacional e de produto que decide **para qual município, secretaria, modelo, política, catálogo e conjunto de assinaturas** aquele motor deverá trabalhar.

A ordem recomendada é:

1. auditar o repositório atual;
2. preservar o fluxo existente;
3. introduzir `tenant_id` e `secretariat_id` sem quebrar dados atuais;
4. implementar resolução de contexto institucional;
5. implementar identidade visual e templates herdáveis;
6. implementar catálogo de cláusulas e motor de políticas;
7. implementar pessoas, vínculos, papéis e assinaturas;
8. conectar tudo ao motor documental do pacote anterior;
9. ativar gradualmente por feature flags.

## Arquivos principais

- `MASTER_PROMPT_IMPLEMENTACAO.md`: instrução completa para a IA com acesso ao repositório.
- `PROMPT_CURTO_PARA_INICIAR.md`: comando resumido para iniciar o trabalho.
- `00_VISAO/`: decisões de produto e limites.
- `01_MULTI_TENANCY/`: isolamento, hierarquia e herança.
- `02_IDENTIDADE_E_MODELOS/`: timbrados, templates e resolução visual.
- `03_POLITICAS_E_CLAUSULAS/`: catálogo imutável e regras condicionais.
- `04_PESSOAS_E_ASSINATURAS/`: pessoas, funções e slots de assinatura.
- `05_UX/`: painéis e jornadas.
- `06_MODELO_DE_DADOS/`: entidades, schemas e contratos.
- `07_API_E_EVENTOS/`: endpoints conceituais e eventos de domínio.
- `08_SEGURANCA/`: isolamento e autorização.
- `09_IMPLEMENTACAO/`: plano incremental, migração e rollback.
- `10_TESTES/`: critérios de aceitação e casos de teste.
- `11_EXEMPLOS/`: exemplos de configuração.
- `12_CONFIG/`: feature flags e políticas iniciais.
- `13_OPERACOES/`: hospedagem, observabilidade e manutenção assistida por IA.

## Restrição essencial

Este pacote é uma especificação de evolução. A IA implementadora deve adaptar nomes, estruturas, ORM, banco, frontend e padrões ao repositório real. Nenhuma recomendação autoriza apagar ou reconstruir módulos funcionais sem prova, migração, testes e rollback.
