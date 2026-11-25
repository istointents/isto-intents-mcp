# Isto Intents — MCP server

Lets ChatGPT or Claude keep watching for a used car or apartment that is not on offer yet and email the user when one appears — MCP server, SK/CZ.

**Isto Intents** is a private intent exchange for used cars and homes — standing purchase intents.
A person tells their AI assistant once what they want; a listing portal in their country watches
used-car and home listings and e-mails them when a matching offer appears. Today it works in
Slovakia and Czechia. Isto Intents is listed in
[ChatGPT's app directory](https://chatgpt.com/plugins/plugin_asdk_app_6add1063e20c8191b4c83f7415bd8c1b)
since 13 November 2026.

The person confirms on the portal's own page before anything is watched. The assistant never
passes a name, an e-mail address or a phone number.

- Server: `https://istointents.com/mcp` — Streamable HTTP, no authentication
- Official MCP Registry: `com.istointents/intents`
- Website and guides for assistants: https://istointents.com/assistants/
- Open protocol, registry of portals and terms: https://istointents.org

This repository describes the hosted server. It does not contain its source code.

## Tools

| tool | what it does |
|---|---|
| `intent_publish_intent` | Passes the person's wish (in their words, with an optional buy-by date and a private maximum price) to a portal that serves their country and has asked to receive wishes. Returns that portal's `consentUrl` — the person must open it, enter their own e-mail and confirm. Nothing is watched before that. |
| `intent_get_intent` | State of a wish, as the portal that holds it reports it. |
| `intent_list_matches` | Listings the portal found for the wish, unchanged; every link leads to the portal's own listing page. |
| `intent_withdraw_intent` | Stops the wish at the portal immediately. |

Portals are chosen only from the [registry](https://istointents.org/registry/) by country and
category, by the rules published at https://istointents.org/demand/#choosing — no portal pays for
receiving wishes or for its place in a list. When no portal serves a country yet, the assistant is
told so plainly and the person can join a waiting list.

## Configure

VS Code (GitHub Copilot) — `.vscode/mcp.json`:

```json
{ "servers": { "istointents": { "type": "http", "url": "https://istointents.com/mcp" } } }
```

or Command Palette → **MCP: Add Server** → HTTP → `https://istointents.com/mcp`.
With Copilot Business or Enterprise an administrator must first enable MCP servers.

Other clients (Claude, ChatGPT, Gemini CLI, Perplexity, Copilot Studio): see
https://istointents.com/assistants/.

## Use

Ask in your own words, for example:

> Let me know until March about an estate car in Portugal under 15 000 €, not diesel.

> Keep an eye out for a 3-room flat in Bratislava up to 250 000 €.

The assistant calls `intent_publish_intent`, shows you the portal's confirmation link, and later
can show what the portal found with `intent_list_matches`. You can stop it at any time, from the
assistant or from the link in every e-mail the portal sends.

## Portals

The four portals behind Isto Intents also run their own search MCP servers, listed on Glama:

- [AutoIsto](https://glama.ai/mcp/connectors/sk.autoisto/used-cars) — used cars, Slovakia
- [AutoIsto CZ](https://glama.ai/mcp/connectors/cz.autoisto/used-cars) — used cars, Czech Republic
- [LivinIsto](https://glama.ai/mcp/connectors/sk.livinisto/real-estate) — homes, Slovakia
- [LivinIsto CZ](https://glama.ai/mcp/connectors/cz.livinisto/real-estate) — homes, Czech Republic

## Privacy

The server forwards only the wish, its optional details and the country — never contact data or
the conversation. What it stores and for how long: https://istointents.com/privacy/

## Contact

Operated by AutoIsto s. r. o., Slovakia · https://istointents.com/contact/
