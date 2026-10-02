# sen-mcp-copilot-connector

The **connector package** that registers Sherpas' [SEN Data](https://sherpas-sen-mcp.web.app)
MCP server as a certified connector for Microsoft Copilot Studio and the
Power Platform. This repository holds only the package artifacts Microsoft's
certification pipeline requires to be open source — not the server, not the
data.

| File | Purpose |
|---|---|
| [`apiDefinition.swagger.json`](apiDefinition.swagger.json) | Swagger 2.0 definition: a single `POST /mcp` operation flagged `x-ms-agentic-protocol: mcp-streamable-1.0`, with the internal `Accept: application/json, text/event-stream` header the server requires (it answers 406 without it). MCP tools are discovered dynamically, so they are not modelled as operations. |
| [`apiProperties.json`](apiProperties.json) | Connector properties: the templated `oauth2generic` identity provider (authorization code + PKCE `S256`, RFC 8707 `resource` indicator, `offline_access`) against Sherpas' authorization server, brand colour, publisher. The client is public (no client secret). `clientId` and `redirectUrl` are placeholders, never guessed: see *Authentication* below. |
| [`intro.md`](intro.md) | The public documentation Microsoft publishes verbatim once the connector is certified. |
| [`icon.png`](icon.png) | 230×230 connector icon. |

## Status

Not yet submitted. Sherpas' Partner Center account is enrolled in the
*Microsoft 365 and Copilot* program; certification is filed once the account
is verified. The server itself is live and already serves Claude and ChatGPT
clients through the same OAuth surface.

## Authentication

The authorization server requires PKCE (`S256`) from every client. The plain
`oauth2` identity provider of Power Platform connectors documents no PKCE
option, so this package uses `oauth2generic` with templates, the pattern of
the certified Highspot MCP connector: the authorization query carries
`code_challenge={CodeChallenge}&code_challenge_method=S256` and the token body
carries `code_verifier={CodeVerifier}`. Both also carry the `resource`
indicator of the MCP endpoint, and the scopes are `openid`,
`https://www.googleapis.com/auth/userinfo.email` and `offline_access`.

The client is registered as a public client: no `client_secret` is sent. Two
values in `apiProperties.json` are placeholders on purpose and are filled when
the connector is created in the Sherpas tenant, never guessed:

- `clientId` (`SET_IN_PARTNER_CENTER`): the client id the authorization server
  returns for the connector's callback.
- `redirectUrl` (`SET_AT_CEA_083_FROM_SECURITY_TAB`): with `GlobalPerConnector`
  the callback has the shape
  `https://global.consent.azure-apim.net/redirect/<connector-internal-name>`
  and exists only once the connector is created (maker portal, *Security* tab).

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
