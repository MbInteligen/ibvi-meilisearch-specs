# Índice: ibvi_properties

**Index name**: `ibvi_properties`  
**Documents**: ~1,447,997  
**Update frequency**: On-demand via indexer  

## Estrutura do Documento

```json
{
  "id": "uuid-string",
  "iptu_code": "12345678901",
  "property_type": "residential|commercial|land|unknown",
  
  "street": "RUA EXEMPLO",
  "number": "123",
  "neighborhood": "JARDIM EUROPA",
  "city": "SAO PAULO",
  "state": "SP",
  "zip_code": "01234000",
  "formatted_address": "RUA EXEMPLO 123, JARDIM EUROPA...",
  
  "land_area_sqm": 500.0,
  "built_area_sqm": 350.0,
  "rooms_count": 4,
  "bathrooms_count": 3,
  "parking_spaces": 2,
  
  "market_value_brl": 2500000.0,
  "price_bucket": "1M-3M",
  
  "_geo": {
    "lat": -23.5505,
    "lng": -46.6333
  }
}
```

## Campos

### Identificação
- **id** (string, required): UUID da propriedade
- **iptu_code** (string, optional): Código IPTU da prefeitura
- **property_type** (string, optional): Tipo do imóvel

### Localização
- **street** (string, optional): Nome da rua
- **number** (string, optional): Número do imóvel
- **neighborhood** (string, optional): Bairro
- **city** (string, optional): Cidade
- **state** (string, optional): Estado (UF)
- **zip_code** (string, optional): CEP
- **formatted_address** (string, optional): Endereço completo formatado

### Características
- **land_area_sqm** (float, optional): Área do terreno em m²
- **built_area_sqm** (float, optional): Área construída em m²
- **rooms_count** (integer, optional): Número de quartos
- **bathrooms_count** (integer, optional): Número de banheiros
- **parking_spaces** (integer, optional): Número de vagas

### Valores
- **market_value_brl** (float, optional): Valor de mercado em BRL
- **price_bucket** (string, optional): Faixa de preço

### Geo
- **_geo.lat** (float, optional): Latitude
- **_geo.lng** (float, optional): Longitude

## Price Buckets

| Bucket | Range |
|--------|-------|
| `0-500k` | R$ 0 - R$ 500.000 |
| `500k-1M` | R$ 500.000 - R$ 1.000.000 |
| `1M-3M` | R$ 1.000.000 - R$ 3.000.000 |
| `3M-6M` | R$ 3.000.000 - R$ 6.000.000 |
| `6M-10M` | R$ 6.000.000 - R$ 10.000.000 |
| `10M+` | > R$ 10.000.000 |

## Configuração do Índice

### Searchable Attributes
```json
[
  "iptu_code",
  "street",
  "neighborhood",
  "city",
  "zip_code",
  "formatted_address"
]
```

### Filterable Attributes
```json
[
  "property_type",
  "city",
  "state",
  "neighborhood",
  "market_value_brl",
  "land_area_sqm",
  "built_area_sqm",
  "price_bucket"
]
```

### Sortable Attributes
```json
[
  "market_value_brl",
  "land_area_sqm",
  "built_area_sqm"
]
```

### Stopwords (PT-BR)
```json
["de", "da", "do", "das", "dos", "em", "na", "no", "nas", "nos", 
 "para", "por", "com", "um", "uma", "uns", "umas", "o", "a", "os", "as", "e", "ou"]
```

### Sinônimos
```json
{
  "cobertura": ["penthouse"],
  "penthouse": ["cobertura"],
  "apartamento": ["apto", "ap"],
  "apto": ["apartamento", "ap"],
  "casa": ["residencia"]
}
```

## Exemplos de Queries

### Busca simples por bairro
```bash
POST /indexes/ibvi_properties/search
{
  "q": "jardim europa"
}
```

### Filtro por faixa de preço e cidade
```bash
POST /indexes/ibvi_properties/search
{
  "q": "apartamento",
  "filter": "price_bucket = '3M-6M' AND city = 'SAO PAULO'"
}
```

### Busca geográfica (5km de raio)
```bash
POST /indexes/ibvi_properties/search
{
  "filter": "_geoRadius(-23.5505, -46.6333, 5000)"
}
```

### Ordenação por valor
```bash
POST /indexes/ibvi_properties/search
{
  "q": "cobertura",
  "filter": "neighborhood = 'JARDIM EUROPA'",
  "sort": ["market_value_brl:desc"],
  "limit": 10
}
```

### Filtro por área construída
```bash
POST /indexes/ibvi_properties/search
{
  "filter": "built_area_sqm >= 200 AND built_area_sqm <= 500"
}
```

### Facet search
```bash
POST /indexes/ibvi_properties/search
{
  "facets": ["price_bucket", "neighborhood", "property_type"]
}
```

## Performance

- **Average search latency**: <50ms (GRU region)
- **Index size**: ~2.5GB
- **RAM usage**: ~2GB (Meilisearch instance)
- **Update time**: ~10-15min (full reindex of 1.4M docs)
