# Skill: Shopify Admin (manage a store)

Gives your Jarvis the ability to read and manage a Shopify store through the Admin GraphQL
API — products, variants, prices, orders, publishing, and store analytics.

Use when the user wants to: list/create/update products, change prices, read orders/revenue,
publish a product, or pull store numbers.

## Read the docs first (source of truth)
- LLM docs: https://shopify.dev/llms.txt
- Admin GraphQL: https://shopify.dev/docs/api/admin-graphql
- (If this Jarvis supports MCP, the official `shopify-dev-mcp` server can author/validate queries.)

## Auth (client-credentials, 24h token)
Env: `SHOPIFY_STORE`, `SHOPIFY_STORE_CLIENT_ID`, `SHOPIFY_STORE_CLIENT_SECRET`, `SHOPIFY_API_VERSION` (default `2025-07`).
1. `POST https://$SHOPIFY_STORE/admin/oauth/access_token` form body
   `grant_type=client_credentials&client_id=...&client_secret=...` → `{ access_token, expires_in }`. Cache ~24h.
2. Use it as `X-Shopify-Access-Token` on `POST /admin/api/$SHOPIFY_API_VERSION/graphql.json`.
Never log the token. Scopes come from how the custom app was installed.

## Example operations (starting points — get the rest from the docs)
- Products: `query{ products(first:20){ nodes{ id title status } } }`
- Create:   `mutation($p:ProductCreateInput!){ productCreate(product:$p){ product{ id } userErrors{ message } } }`
- Price:    `mutation($pid:ID!,$v:[ProductVariantsBulkInput!]!){ productVariantsBulkUpdate(productId:$pid,variants:$v){ userErrors{ message } } }`
- Orders/revenue: `query($q:String!){ orders(first:250,query:$q){ nodes{ currentTotalPriceSet{ shopMoney{ amount } } } } }`
- Analytics (needs `read_reports`): `shopifyqlQuery(query:"...")`

## Build it into this Jarvis (Eve conventions)
1. `agent/skills/shopify-admin/SKILL.md` — frontmatter `name: shopify-admin` + when-to-use + auth + the gql call shape + "read https://shopify.dev/docs/api/admin-graphql for the full schema."
2. `agent/tools/shopify_admin.ts` — `import { defineTool } from "eve/tools"`; a cached token mint + a generic `gql<T>()` helper, then a small set of tools (`shopify_products`, `shopify_orders`, `shopify_create_product`, `shopify_set_price`), secrets via `process.env`. Read the docs for exact field names.
3. Match this repo's conventions; never hardcode secrets.
