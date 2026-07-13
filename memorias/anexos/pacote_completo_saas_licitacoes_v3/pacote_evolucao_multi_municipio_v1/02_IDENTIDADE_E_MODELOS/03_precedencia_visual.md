# Precedência visual

Ordem recomendada:

1. override do tipo documental na secretaria;
2. identidade específica da secretaria;
3. override do tipo documental no município;
4. identidade padrão do município;
5. padrão seguro da plataforma.

O sistema deve exibir ao proprietário a origem de cada valor resolvido, por exemplo:

```text
logo: Secretaria de Infraestrutura
rodapé: Município de Paragominas
fonte: padrão da plataforma
margens: modelo TR v3
```

Conflitos devem ser bloqueados antes da geração, não descobertos depois no PDF.
