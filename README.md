# sen-mcp-copilot-connector

The **connector package** that registers Sherpas' [SEN Data](https://sherpas-sen-mcp.web.app)
MCP server as a certified connector for Microsoft Copilot Studio and the
Power Platform. This repository holds only the package artifacts Microsoft's
certification pipeline requires to be open source — not the server, not the
data.

| File | Purpose |
|---|---|
| [`apiDefinition.swagger.json`](apiDefinition.swagger.json) | Swagger 2.0 definition: a single `POST /mcp` operation flagged `x-ms-agentic-protocol: mcp-streamable-1.0`. MCP tools are discovered dynamically, so they are not modelled as operations. |
| [`apiProperties.json`](apiProperties.json) | Connector properties: generic OAuth 2.0 (authorization code + PKCE) against Sherpas' authorization server, brand colour, publisher. The client id is set in Partner Center, never here. |
| [`intro.md`](intro.md) | The public documentation Microsoft publishes verbatim once the connector is certified. |
| [`icon.png`](icon.png) | 230×230 connector icon. |

## Status

Not yet submitted. Sherpas' Partner Center account is enrolled in the
*Microsoft 365 and Copilot* program; certification is filed once the account
is verified. The server itself is live and already serves Claude and ChatGPT
clients through the same OAuth surface.

## Try it in your own tenant before certification

In Copilot Studio: **Tools → Add a tool → New tool → Model Context
Protocol**, server URL `https://sen-mcp-394946609261.us-central1.run.app/mcp`,
authentication **OAuth 2.0 → Dynamic discovery**. The wizard displays the
callback URL it will use; Sherpas must allowlist that URL on the authorization
server before the first connection succeeds (redirect URIs are never
inferred). An admitted account is required — request access on the trust page.

## Certification path

The package is submitted through Partner Center (*Marketplace offers →
Microsoft 365 and Copilot → New offer → Connectors & Agents in Microsoft
Copilot Studio*) and open-sourced through a pull request to
[`microsoft/PowerPlatformConnectors`](https://github.com/microsoft/PowerPlatformConnectors).
This repository is the canonical home of the package; the PR mirrors it.

## Licence

MIT — see [`LICENSE`](LICENSE). Applies to the package files only. The SEN
Data service is governed by its own
[terms](https://sherpas-sen-mcp.web.app/terms) and
[privacy policy](https://sherpas-sen-mcp.web.app/privacy); the underlying data
originates with Chile's Coordinador Eléctrico Nacional.
