# Hierarquia e herança de configurações

## Níveis

1. padrão da plataforma;
2. padrão do município;
3. sobrescrita da secretaria;
4. sobrescrita do tipo documental;
5. snapshot específico do documento.

## Ordem de resolução

A configuração mais específica vence, desde que o campo permita sobrescrita:

```text
document_snapshot > document_type_override > secretariat > tenant > platform_default
```

## Campos bloqueáveis

O proprietário da plataforma poderá marcar configurações como:

- herdável;
- sobrescrevível pela secretaria;
- exclusiva do proprietário;
- obrigatória;
- bloqueada.

## Snapshot

No início da geração, o sistema deve produzir um `ResolvedInstitutionalContext` contendo todos os valores já resolvidos. Esse snapshot é associado à versão documental para garantir reprodutibilidade.

Mudanças posteriores no timbrado, secretário ou cláusula não alteram documentos já emitidos.
