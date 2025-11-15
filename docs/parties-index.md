# Índice: ibvi_parties

> **Status:** Em planejamento. Este documento descreve o formato esperado para clientes, owners e parceiros comerciais que participarão do grafo de relacionamento do IBVI.

## Objetivo

- Permitir busca rápida por pessoas envolvidas em um imóvel (proprietários, corretores, investidores).
- Disponibilizar filtros por papel (`role`) e localização.
- Servir de base para features de CRM e compliance.

## Estrutura proposta

```json
{
  "id": "74397662-29a8-4fb5-9f03-5dd2f6528fb0",
  "external_id": "crm_88921",
  "full_name": "Maria Fernanda Lima",
  "alias": "Maria Lima",
  "email": "maria.lima@example.com",
  "phone": "+55 21 99999-8888",
  "document": "***.456.789-**",
  "role": "owner",
  "city": "Rio de Janeiro",
  "state": "RJ",
  "country": "BR",
  "tags": ["vip", "priority"],
  "updated_at": "2024-07-20T09:00:00Z",
  "metadata": {
    "source": "salesforce",
    "manager_id": "a13bc3d2"
  }
}
```

## Searchable attributes

- `full_name`
- `alias`
- `email`
- `phone`
- `tags`

## Filterable attributes

- `role` (enum: `owner`, `buyer`, `broker`, `investor`, `partner`)
- `city`
- `state`
- `country`
- `tags`
- `metadata.source`

## Sortable attributes

- `role`
- `updated_at`

## Requisitos de privacidade

- Dados sensíveis (`document`, `email`, `phone`) devem ser mascarados conforme LGPD antes da indexação caso o contato opte por não ser contatado.
- Apenas papéis aprovados pelo jurídico podem ser expostos publicamente.

## Próximos passos

1. Confirmar conjunto mínimo de campos obrigatórios com o time jurídico.
2. Adicionar a flag `consent_opt_in` ao schema e documentar o comportamento.
3. Integrar com o reindexador quando o dataset estiver maduro.
