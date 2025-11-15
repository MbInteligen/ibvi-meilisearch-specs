# Índice: ibvi_addresses

## Resumo

| Chave | Valor |
| --- | --- |
| Nome do índice | `ibvi_addresses` |
| Fonte | `ibvi_addresses` + `ibvi_properties` (join opcional) |
| Responsável | Squad Data & Search |
| Objetivo | Fornecer autosuggest de endereços e permitir filtros geográficos |

## Estrutura do documento

```json
{
  "id": "b4d08e73-9a21-4df4-bd92-b21a6f19d878",
  "property_id": "c1f71c3c-8fbc-4d1a-84e8-74b778df2629",
  "display_name": "Rua das Flores, 123 - Jardim Europa, São Paulo",
  "line1": "Rua das Flores, 123",
  "city": "São Paulo",
  "state": "SP",
  "country": "BR",
  "postal_code": "01456-010",
  "geo_lat": -23.570188,
  "geo_lng": -46.67466,
  "accuracy": "rooftop",
  "updated_at": "2024-08-02T10:14:00Z",
  "metadata": {
    "zone": "zona-sul",
    "ibge": "3550308"
  }
}
```

## Searchable attributes

1. `display_name`
2. `line1`
3. `city`
4. `postal_code`

## Filterable attributes

- `city`
- `state`
- `country`
- `postal_code`
- `accuracy`
- `metadata.zone`

## Sortable attributes

- `city`
- `state`
- `updated_at`

## Facetas

- `city`
- `state`
- `accuracy`

## Notas de implementação

- `display_name` é montado pelo indexador concatenando `line1`, bairro e cidade.
- Coordenadas (`geo_lat`, `geo_lng`) são opcionais, mas quando presentes habilitam ordenação por distância na API do Meilisearch (via `_geo` quando migrarmos para esse formato).
- Endereços duplicados devem ser consolidados pelo `property_id` + `postal_code` antes da publicação.

## Evoluções planejadas

- Guardar `_geo` com tipo `{ "lat": <float>, "lng": <float> }`.
- Adicionar flag `is_verified` para endereços auditados manualmente.
