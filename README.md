# Healthcare Provider NPI Enricher MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-healthcare-provider-npi-enricher/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-healthcare-provider-npi-enricher) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-healthcare-provider-npi-enricher%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-healthcare-provider-npi-enricher&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-healthcare-provider-npi-enricher)](https://www.npmjs.com/package/@mambalabsdev/mcp-healthcare-provider-npi-enricher) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-healthcare-provider-npi-enricher)](https://www.npmjs.com/package/@mambalabsdev/mcp-healthcare-provider-npi-enricher) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-healthcare-provider-npi-enricher)](https://github.com/mambalabsdev/mcp-healthcare-provider-npi-enricher/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-healthcare-provider-npi-enricher)

An MCP server for the Mamba Labs Healthcare Provider NPI Enricher actor on Apify. Turn NPIs or provider names into NPI Registry records.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give NPIs, individual names, organization names, or a taxonomy with a state. The actor reads the official US NPI Registry and returns one row per provider with taxonomy, license, practice address and phone, other identifiers, and organization official. It returns only what the registry publishes. An invalid NPI, a lookup with no match, and a summary row are free.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-healthcare-provider-npi-enricher": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-healthcare-provider-npi-enricher"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `enrich_healthcare_providers_npi` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Look up NPI 1003000126."
- "Find cardiovascular disease specialists in Boston in the NPI Registry."
- "Enrich these provider names in California with taxonomy and practice phone."

## Tool

`enrich_healthcare_providers_npi`. One tool. Example arguments:

```json
{
  "npis": [
    "1003000126"
  ],
  "organization_names": [
    "mayo clinic"
  ],
  "state": "MN",
  "max_results_per_lookup": 5
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `npis` | array | no | 10 digit NPIs. Each returns its record. An invalid NPI or one with no record is a free row. |
| `individual_names` | array | no | People, as Jane Doe, Doe, Jane, or Dr. Jane Doe MD. The registry also matches common first name aliases. A trailing * after 2 or more letters is a wildcard. Add state to narrow common names. |
| `organization_names` | array | no | Organizations. The registry matches whole words, so add a trailing *, such as kaiser*, to widen the match. |
| `taxonomy` | string | no | Filter or search by the registry's taxonomy wording, such as cardiovascular disease, or cardio* as a wildcard. Cardiology alone matches only Pharmacist, Cardiology. Applies to names and to the search. Does not apply to NPIs. |
| `state` | string | no | Two letter state code. Applies to names and to the search. The registry refuses state alone, so give it with a name, taxonomy, city, or postal code. |
| `city` | string | no | City filter. Applies to names and to the search. |
| `postal_code` | string | no | 5 or 9 digit postal code, with a trailing * after 2 or more digits as a wildcard. |
| `provider_type` | string | no | For the taxonomy, city, or postal code search: both (default), individual, or organization. |
| `max_results_per_lookup` | number | no | Provider rows per name or search, from 1 to 1200. Default 25. The registry serves at most 1200 records per search. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `provider-row`, one provider record | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

Lookup summaries, NPIs not found, invalid NPIs, and refused input are free. Mamba Labs pays the platform costs of the run. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/healthcare-provider-npi-enricher](https://apify.com/mambalabs/healthcare-provider-npi-enricher)

## License

MIT. See [LICENSE](LICENSE).
