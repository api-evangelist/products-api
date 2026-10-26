# Products API (api-evangelist-products)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
