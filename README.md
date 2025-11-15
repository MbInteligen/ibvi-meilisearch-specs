# IBVI Meilisearch Specs

📖 **Public repository** – Documento vivo com as especificações oficiais dos índices do Meilisearch utilizados pelo IBVI.

## Objetivo

Centralizar as definições (schema, ranking rules, filtros e convenções) que precisam ser sincronizadas entre engenharia, dados e produto. Cada mudança descrita aqui deve ser acompanhada por uma alteração equivalente no reindexador (`ibvi-meilisearch-indexer`).

## Estrutura

- `docs/properties-index.md` – especificações para `ibvi_properties`
- `docs/addresses-index.md` – especificações para `ibvi_addresses`
- `docs/parties-index.md` – blueprint inicial para `ibvi_parties`

## Workflow sugerido

1. Proponha alterações através de pull requests descrevendo o _rationale_.
2. Atualize o reindexador com o novo schema antes de publicar.
3. Gere um reindex completo no ambiente de staging e valide as buscas principais.
4. Promova para produção quando o resultado estiver aprovado pelo time de produto.

## Convenções gerais

- Todos os índices usam sintaxe `ibvi_<entidade>`.
- Os campos `id` são `UUID v4` em todos os datasets.
- Todos os carimbos de data/hora usam UTC (`TIMESTAMPTZ`).
- Filtros booleanos devem obedecer ao padrão `*_flag`.
- Sempre que houver campos com conteúdo sensível (ex.: documentos pessoais) eles devem ser mascarados ou omitidos antes de chegar ao Meilisearch.

## Repositórios relacionados

- `ibvi-meilisearch-indexer` (privado): reindexador em Rust que materializa estes esquemas.
- `ibvi-meilisearch-specs` (público): este repositório de referência.
