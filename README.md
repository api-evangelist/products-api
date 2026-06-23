# Products API (api-evangelist-products)

A template APIs.json for a products API, used in storytelling, training, and knowledge
bases. It is intentionally boring — list, create, read, update, delete, and cancel a
product — so it can serve as a clean canvas for demonstrating API artifacts and the
relationships between them.

**APIs.json:** [`apis.yml`](apis.yml)

## Repository layout

```
apis.yml                      # APIs.json index — registers every artifact below
openapi/                      # canonical OpenAPI 3.1 description
collections/                  # Postman + Open Collection
schema/                       # standalone JSON Schemas (+ index)
overlays/                     # locale-first overlays for the OpenAPI and schemas
finops/  plans/  rate-limits/ # operational artifacts
```

## Artifacts

| Type | File |
|------|------|
| OpenAPI | [`openapi/products-api-openapi.yml`](openapi/products-api-openapi.yml) |
| Postman Collection | [`collections/products-api.postman_collection.json`](collections/products-api.postman_collection.json) |
| Open Collection | [`collections/products-api.opencollection.json`](collections/products-api.opencollection.json) |
| JSON Schema (index) | [`schema/index.yml`](schema/index.yml) |
| JSON Schemas | `schema/Product.yml`, `Meta.yml`, `Problem.yml`, `LinksSelf.yml`, `LinksPagination.yml`, `WrapperResponseObject.yml`, `WrapperResponseCollection.yml` |
| FinOps | [`finops/products-api-finops.yml`](finops/products-api-finops.yml) |
| Plans | [`plans/products-api-plans.yml`](plans/products-api-plans.yml) |
| Rate Limits | [`rate-limits/products-api-rate-limits.yml`](rate-limits/products-api-rate-limits.yml) |

Every one of these is registered as a property in [`apis.yml`](apis.yml) so the repo is
discoverable as a single machine-readable index.

## Schemas

The data models do not live inside the OpenAPI document — they are broken out into
standalone JSON Schema files under [`schema/`](schema/), each with its own `$id` and each
valid on its own. [`schema/index.yml`](schema/index.yml) `$ref`s them together. Keeping the
models standalone means the same `Product.yml` can be referenced by the OpenAPI, a
validation pipeline, a code generator, and a documentation site without any one of those
purposes owning the definition.

## Overlays

[Overlays](https://spec.openapis.org/overlay/latest.html) apply targeted changes on top of a
base document **without forking it**. They are organized **locale-first** — one folder per
language, with the OpenAPI overlay and the schema overlays for that language together:

```
overlays/
  de/                         # German
    openapi.yml               # overlay for openapi/products-api-openapi.yml
    schema/
      Product.yml             # overlay for schema/Product.yml
      Meta.yml
      ...
  fr/  it/  nl/               # French, Italian, Dutch (same shape)
```

Each overlay `extends` the canonical raw URL of the artifact it modifies on the `main`
branch, and only rewrites human-readable fields (summaries, descriptions, error messages) —
never the structure. The base is the single source of truth; an overlay is a presentation
layer over it.

### Applying an overlay

Using the [openapi-overlays](https://www.npmjs.com/package/openapi-overlays) CLI (or any
Overlay-aware tool):

```sh
# produce the German OpenAPI document
npx openapi-overlays apply \
  openapi/products-api-openapi.yml \
  overlays/de/openapi.yml > products-api-openapi.de.yml

# produce the German Product schema
npx openapi-overlays apply \
  schema/Product.yml \
  overlays/de/schema/Product.yml > Product.de.yml
```

## Tags

- Application Programming Interface
- Products

## Maintainers

**API Evangelist** — info@apievangelist.com · [apievangelist.com](http://apievangelist.com)

## License

Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC-BY-NC-SA-4.0).
