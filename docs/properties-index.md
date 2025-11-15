# Índice: ibvi_properties

## Resumo

| Chave | Valor |
| --- | --- |
| Namespace | `ibvi` |
| Nome do índice | `ibvi_properties` |
| Fonte de dados | Tabela `ibvi_properties` no PostgreSQL (Neon) |
| Responsável | Squad Data & Search |
| Frequência de reindex | On-demand (GitHub Actions → Fly.io) |

## Estrutura do documento

```json
{
  "id": "c1f71c3c-8fbc-4d1a-84e8-74b778df2629",
  "slug": "apartamento-vista-mar-natal",
  "title": "Apartamento vista mar em Natal",
  "description": "Cobertura com 3 suítes, 2 vagas e vista definitiva para o mar.",
  "city": "Natal",
  "state": "RN",
  "country": "BR",
  "neighborhood": "Ponta Negra",
  "price": 1450000,
  "currency": "BRL",
  "bedrooms": 3,
  "bathrooms": 4,
  "parking_spots": 2,
  "area_m2": 198,
  "status": "for_sale",
  "is_highlight": true,
  "tags": ["vista_mar", "cobertura"],
  "amenities": ["pool", "elevator", "gated"],
  "updated_at": "2024-08-04T12:17:45Z",
  "metadata": {
    "builder": "IBVI",
    "reference_code": "IB-98233"
  }
}
```

## Campos obrigatórios

| Campo | Tipo | Origem | Observações |
| --- | --- | --- | --- |
| `id` | `uuid` | `ibvi_properties.id` | Primary key / `primaryKey` no Meilisearch |
| `slug` | `text` | `slug` | Usado para deep links no site |
| `title` | `text` | `title` | Resumo curto exibido nas listagens |
| `city` | `text` | `city` | Normalizado em Title Case |
| `state` | `text` | `state` | Sigla de 2 letras |
| `country` | `text` | `country` | Sempre `BR` por enquanto |
| `status` | `text` | `status` | Enum: `for_sale`, `for_rent`, `sold`, `inactive` |
| `price` | `number` | `price_cents / 100` | Em moeda inteira |
| `updated_at` | `datetime` | `updated_at` | ISO-8601, UTC |

## Campos opcionais

`description`, `neighborhood`, `bedrooms`, `bathrooms`, `parking_spots`, `area_m2`, `is_highlight`, `tags`, `amenities`, `metadata`

## Searchable attributes

1. `title`
2. `description`
3. `city`
4. `neighborhood`
5. `tags`

## Filterable attributes

- `status`
- `city`
- `state`
- `country`
- `bedrooms`
- `bathrooms`
- `parking_spots`
- `area_m2`
- `price`
- `is_highlight`
- `tags`
- `amenities`

## Sortable attributes

- `price`
- `area_m2`
- `bedrooms`
- `bathrooms`
- `updated_at`

## Facetas recomendadas

- `city`
- `state`
- `bedrooms`
- `bathrooms`
- `amenities`

## Ranking rules customizadas

1. `words`
2. `typo`
3. `proximity`
4. `attribute`
5. `sort`
6. `exactness`
7. **Regra customizada**: `desc(is_highlight)` (dá prioridade a vitrines)
8. **Regra customizada**: `asc(price)` (preferir imóveis mais acessíveis quando empate)

## Regras de negócio

- `status = inactive` não deve ser indexado (filtrar na query SQL do indexador).
- `price` precisa ser normalizado para a moeda atual; armazenamos `currency` para queries futuras.
- `tags` e `amenities` devem estar em `snake_case`.

## Checklist pós mudança

- Atualize `src/main.rs` no indexador com novos campos.
- Gere um dump do índice de staging e compare com a especificação.
- Abra issue pública descrevendo a alteração para parceiros de integração.
