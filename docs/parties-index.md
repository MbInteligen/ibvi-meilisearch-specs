# Índice: ibvi_parties

**Index name**: `ibvi_parties`  
**Status**: 🚧 Em desenvolvimento  

## Estrutura Planejada

```json
{
  "id": "uuid-string",
  "name": "João da Silva",
  "party_type": "person|company",
  "document": "12345678901",
  "emails": ["joao@example.com"],
  "phones": ["+5511999999999"],
  "city": "SAO PAULO",
  "state": "SP",
  "neighborhood": "JARDIM EUROPA",
  "tags": ["owner", "vip", "developer"]
}
```

## Campos Planejados

- **id**: UUID da party
- **name**: Nome completo ou razão social
- **party_type**: `person` ou `company`
- **document**: CPF/CNPJ normalizado
- **emails**: Array de emails
- **phones**: Array de telefones
- **city, state, neighborhood**: Localização principal
- **tags**: Classificações (owner, buyer, vip, etc)

## Configuração Planejada

### Searchable
```json
["name", "document", "emails", "phones", "city"]
```

### Filterable
```json
["party_type", "city", "state", "tags"]
```

## Status

Aguardando SQL query para implementação do indexador.
