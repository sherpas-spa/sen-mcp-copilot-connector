# SEN Data

SEN Data is a remote MCP (Model Context Protocol) server operated by
[Sherpas Group SpA](https://sherpas-sen-mcp.web.app) that lets an AI agent
query **certified, governed data about Chile's national electricity system**
(SEN — Sistema Eléctrico Nacional): generation, demand, marginal cost (CMg),
curtailment, battery storage (BESS) and commercial settlement. Every figure
is computed by deterministic, certified machinery over a BigQuery-backed
warehouse, resolved to one canonical identity per plant, substation and
company, and carries its data-coverage date.

## Publisher

Sherpas Group SpA — Santiago, Chile. Support: <jorge@sherpas.net>.

## Prerequisites

- An account admitted to the SEN Data alpha. The alpha is **free** and
  **access is granted by individual request**: fill in the form at
  <https://sherpas-sen-mcp.web.app> and we answer within one business day.
  There is no self-service sign-up.
- A Microsoft Copilot Studio agent with generative orchestration turned on.

## How to obtain credentials

No API key is involved. The connector uses **OAuth 2.0 (authorization code
with PKCE)** against Sherpas' own authorization server; you sign in with the
Google Workspace account that was admitted to the alpha. The server
advertises standard discovery documents
(`/.well-known/oauth-authorization-server`), supports dynamic client
registration, and refuses any identity that is not on the admitted list.

## Supported operations

The connector exposes a single Streamable HTTP endpoint (`POST /mcp`); the
tools below are discovered dynamically from the server and kept in sync
automatically.

| Tool | What it does |
|---|---|
| `sen_list_metrics` | Lists the certified metrics and the analytical operation catalog (measures × dimensions × operations). |
| `sen_get_metric` | Returns the definition, operative choices and provenance of one certified metric. |
| `sen_run_metric` | Executes a certified metric with ratified operative definitions and returns the figures with provenance and coverage window. |
| `sen_run_operation` | Runs an analytical shape (ranking, share, delta, top-N…) over a certified measure. |
| `sen_resolve_entity` | Resolves an ambiguous plant, substation or company name to its canonical identifier. |
| `sen_query_sql` | Last-resort, read-only SQL over the published tables; results are explicitly marked as non-certified. |
| `sen_data_status` | Reports the freshness and coverage cut-off of the data behind each metric. |
| `sen_submit_report` | Submits a conversation report to Sherpas — only when the user explicitly asks for one. |

The server also publishes the `sen://limitations` resource: the declared data
gaps and freshness limits, identical to the public trust page.

## Known issues and limitations

- **Alpha, by request.** Admission is individual; Sherpas may decline a
  request or discontinue the alpha. Coverage of the full data catalog is
  partial and **declared, never hidden**: see the trust page and the
  `sen://limitations` resource for the exact tables, coverage dates and the
  reason behind every gap.
- **Usage ceiling.** MCP queries share a technical budget of billed BigQuery
  bytes per hour. When the budget is exhausted, requests are refused with an
  explicit message and resume the next hour — they are never silently
  degraded.
- **Exploratory SQL** (`sen_query_sql`) is read-only, limited to published
  tables, capped in bytes and rows, and its results are de-certified; prefer
  the certified metrics.
- **Language.** The underlying data and many labels are in Spanish (the
  language of the source, Chile's Coordinador Eléctrico Nacional).
- **No SLA.** Best-effort availability; no uptime commitment during the alpha.

## Frequently asked questions

**Is it free?** Yes, during the alpha. No paid tier exists.

**Where is it available?** Worldwide. Access is gated by admission, not by
location.

**Where does the data come from?** Chile's Coordinador Eléctrico Nacional
(CEN), which publishes it under a legal duty. Attribute the CEN for the
underlying data and Sherpas for the certified layer (licensed CC-BY 4.0 to the
authenticated audience).

**Do you see my conversation?** No. MCP sends the server only the structured
tool call, never your prompt. Details in the
[privacy policy](https://sherpas-sen-mcp.web.app/privacy).

## Documentation and legal

- Trust page and access request: <https://sherpas-sen-mcp.web.app>
- Privacy policy: <https://sherpas-sen-mcp.web.app/privacy>
- Terms of service: <https://sherpas-sen-mcp.web.app/terms>
