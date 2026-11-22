# AI-TENDER — Belarus Open Public Procurements (MCP & Open API)

[![mcptry](https://mcptry.com/badge/by.ai-tender/tenders)](https://mcptry.com/servers/by.ai-tender/tenders)

Public **Model Context Protocol (MCP)** server and **OpenAPI** specification for **[AI-TENDER (ai-tender.by)](https://ai-tender.by)** — structured access to open public procurements of Belarus aggregated from official procurement platforms (`goszakupki.by`, `icetrade.by`, `gias.by`).

> **Этот репозиторий содержит только публичные спецификации и документацию входа MCP/OpenAPI.** Исходный код сервиса и внутренние маршруты здесь не публикуются.

## Epistemic Guarantees / Принципы доказательности

- **Strict public equivalence:** MCP tools return only the open procurements (submission deadline not passed) already published on `/zakupki.php`, with direct links to each official source platform. Client accounts, profiles, and workspace analyses are never exposed.
- **Explicit unknowns (`null` / `found=false`):** Unspecified currency stays `null` (never defaults silently to `BYN`), deadlines without a clock hour carry `time: null` with an explicit `time_note`, and closed or unknown tenders return `found: false` with an explicit `meaning`.
- **Facts only — no synthetic scores:** No win probabilities, supplier ratings, recommended prices, or bid recommendations.

## Read-Only MCP Tools (`https://ai-tender.by/mcp`)

| Tool | Mode | Description |
|---|---|---|
| `search_tenders` | read-only | Search open public procurements of Belarus (deadline not passed) by title/customer words or customer 9-digit UNP (`query`, optional `region`, `limit` 1–25). |
| `get_tender` | read-only | Retrieve one open procurement card by id (`<source>:<number>`, e.g. `goszakupki:3653566`, `icetrade:1372121`). Closed or unknown ids return `found=false`. |
| `list_regions` | read-only | List Belarus regions with the number of currently open procurements in each. |

## Quick Start

### Claude Code

```bash
claude mcp add --transport http tenders https://ai-tender.by/mcp
```

### Cursor / Claude Desktop / Windsurf (`mcp.json`)

```json
{
  "mcpServers": {
    "ai-tender": {
      "type": "streamable-http",
      "url": "https://ai-tender.by/mcp"
    }
  }
}
```

### Direct JSON-RPC (`curl`)

```bash
curl -s -X POST https://ai-tender.by/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```

## Public Machine Endpoints

- **MCP endpoint (`streamable-http`):** `https://ai-tender.by/mcp`
- **MCP Registry entry:** [`server.json`](server.json) (`by.ai-tender/tenders`)
- **OpenAPI specification (3.1):** [`openapi.json`](openapi.json) · live at [`https://ai-tender.by/openapi.json`](https://ai-tender.by/openapi.json)
- **LLM index:** [`llms.txt`](llms.txt) · live at [`https://ai-tender.by/llms.txt`](https://ai-tender.by/llms.txt)
- **Open procurements web catalogue:** [`https://ai-tender.by/zakupki.php`](https://ai-tender.by/zakupki.php)
- **Service status & source freshness:** [`https://ai-tender.by/status.php`](https://ai-tender.by/status.php) ([JSON](https://ai-tender.by/status.php?format=json))
- **Counterparty KYB checks by UNP:** Companion MCP server **[AI-BELARUS](https://github.com/Apex-prim/ai-belarus-mcp)** at `https://ai-belarus.by/mcp`

## Operator & Legal Notice

Operated by **ООО «Тендерные машины»** (Minsk, Belarus).
Re-distribution of procurement records remains subject to official source platform attribution and Belarus Law No. 99-Z on Personal Data Protection.
