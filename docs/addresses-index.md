# Índice: ibvi_addresses

**Index name**: `ibvi_addresses`  
**Documents**: ~2M+  
**Update frequency**: On-demand via indexer  

## Estrutura do Documento

```json
{
  "id": "uuid-string",
  "street": "RUA EXEMPLO",
  "street_type": "RUA",
  "number": "123",
  "neighborhood": "JARDIM EUROPA",
  "city": "SAO PAULO",
  "state": "SP",
  "zip_code": "01234000",
  "complement": "APTO 45",
  "formatted_address": "RUA EXEMPLO 123, JARDIM EUROPA, SAO PAULO - SP, CEP 01234000",
  "_geo": {
    "lat": -23.5505,
    "lng": -46.6333
  }
}
```

## Campos

- **id** (string, required): UUID do endereço
- **street** (string, optional): Nome da rua
- **street_type** (string, optional): Tipo de logradouro (RUA, AV, etc)
- **number** (string, optional): Número
- **neighborhood** (string, optional): Bairro
- **city** (string, optional): Cidade
- **state** (string, optional): Estado (UF)
- **zip_code** (string, optional): CEP
- **complement** (string, optional): Complemento
- **formatted_address** (string, optional): Endereço formatado
- **_geo** (object, optional): Coordenadas geográficas

## Configuração

### Searchable Attributes
```json
["street", "neighborhood", "city", "zip_code", "formatted_address"]
```

### Filterable Attributes
```json
["city", "state", "neighborhood"]
```

### Stopwords (PT-BR)
```json
["de", "da", "do", "das", "dos", "em", "na", "no", "rua", "avenida", "travessa"]
```

## Exemplos de Queries

### Busca por CEP
```bash
POST /indexes/ibvi_addresses/search
{
  "q": "01310100"
}
```

### Busca por rua
```bash
POST /indexes/ibvi_addresses/search
{
  "q": "av paulista",
  "filter": "city = 'SAO PAULO'"
}
```

### Autocomplete de endereço
```bash
POST /indexes/ibvi_addresses/search
{
  "q": "rua oscar fre",
  "limit": 10
}
```
