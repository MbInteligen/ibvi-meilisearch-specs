# IBVI Meilisearch Specs

📖 **Public repository** - Especificações dos índices Meilisearch do IBVI.

## Sobre

Documentação pública dos índices de busca mantidos pelo IBVI usando **Meilisearch**.

Este repositório contém:
- Estrutura dos documentos (properties, addresses, parties)
- Convenções de nomes e filtros
- Exemplos de queries e ordenação
- Boas práticas para busca em português (stopwords, sinônimos)

## Índices

### 🏠 Properties (`ibvi_properties`)
**~1.4M documentos**

Propriedades imobiliárias em São Paulo com dados de IPTU, valores de mercado e geocoding.

[📄 Documentação completa](docs/properties-index.md)

### 📍 Addresses (`ibvi_addresses`)
**~2M+ documentos**

Base de endereços brasileiros com normalização, geocoding e validação.

[📄 Documentação completa](docs/addresses-index.md)

### 👥 Parties (`ibvi_parties`)
**Em desenvolvimento**

Pessoas físicas e jurídicas com dados de contato e relacionamentos.

[📄 Documentação completa](docs/parties-index.md)

## Quick Start

### Exemplo: Buscar apartamentos em Jardim Europa

```bash
curl -X POST 'https://your-meili.ibvi.com/indexes/ibvi_properties/search' \
  -H 'Authorization: Bearer YOUR_SEARCH_KEY' \
  -H 'Content-Type: application/json' \
  --data-binary '{
    "q": "jardim europa apartamento",
    "filter": "price_bucket = \"3M-6M\" AND city = \"SAO PAULO\"",
    "limit": 20,
    "sort": ["market_value_brl:desc"]
  }'
```

### Exemplo: Busca geográfica (raio)

```bash
curl -X POST 'https://your-meili.ibvi.com/indexes/ibvi_properties/search' \
  -H 'Authorization: Bearer YOUR_SEARCH_KEY' \
  -H 'Content-Type: application/json' \
  --data-binary '{
    "filter": "_geoRadius(-23.5505, -46.6333, 5000)",
    "limit": 50
  }'
```

## Português

Todos os índices estão otimizados para busca em português:
- **Stopwords**: de, da, do, em, na, para, etc.
- **Sinônimos**: cobertura↔penthouse, apartamento↔apto
- **Typo tolerance**: Ativada por padrão
- **Accent handling**: Normalização automática

## Tecnologia

- **Search Engine**: [Meilisearch](https://www.meilisearch.com/) v1.11+
- **Database**: PostgreSQL 17 (Neon)
- **Indexer**: Rust (privado)
- **Hosting**: Fly.io (gru region)

## Links

- [Meilisearch Documentation](https://www.meilisearch.com/docs)
- [IBVI Website](https://ibvi.com.br)
- Indexer code: 🔒 Private repository

## License

Specifications: MIT  
Data: © IBVI - All rights reserved
