# A2A Cosmetics — connect kit

> Agent-to-Agent (A2A) + Model Context Protocol (MCP) hub for cosmetics.

Business to business: brands, manufacturers, retailers, distributors and their AI agents. Eighteen markets, thirteen rule sets; every answer names the market, the rule set and the version it was judged against. Operated by GreenCore Solutions Corp. Public connect kit, MIT.

Notice: Artificial intelligence makes mistakes. A2A Cosmetics is an information source, not a recommendation. Rules differ by jurisdiction — refer to the regulator in your jurisdiction for the information that applies to you.

## The door

<!-- door:begin -->streamable-HTTP, stateless, server name `a2a-cosmetics`, door version 1.0.0, 20 tools (read from the wire 2026-09-20)<!-- door:end -->

- MCP: `https://mcp.a2a-cosmetics.ai/mcp` — any client that speaks streamable-HTTP: `{ "url": "https://mcp.a2a-cosmetics.ai/mcp", "transport": "streamable-http" }`
- A2A 0.3.0 JSON-RPC: `https://a2a-cosmetics.ai/a2a` (the hub) and `https://<cc>.a2a-cosmetics.ai/a2a` (one market agent per market, answered from its own Azure region)
- Agent Card: `https://a2a-cosmetics.ai/.well-known/agent-card.json` (ES256, kid `a2ac-2026-09`; keyring `/.well-known/jwks.json`)
- Setup: https://a2a-cosmetics.ai/setup · Docs: https://a2a-cosmetics.ai/docs · Claim your record: https://a2a-cosmetics.ai/claim
- Official MCP Registry: `io.github.greencore-solutions/a2a-cosmetics` (server.json in this repo)

## The gate

Three calls before any price: `resolve_jurisdiction` → `resolve_actor` → `gate_transaction`. Twelve reason codes: ALLOW · REQUIRE_NOTIFICATION · REQUIRE_RESPONSIBLE_PERSON · DENY_NOT_A_COSMETIC_HERE · DENY_INGREDIENT_BANNED · DENY_INGREDIENT_LIMIT · DENY_CLAIM · DENY_MARKET · DENY_ACTOR_CLASS · DENY_UNLICENSED_AGENT · DENY_NO_GTIN · DENY_NOT_VERIFIED.

## The twenty tools

- **Gate** — `resolve_jurisdiction`, `resolve_actor`, `gate_transaction`
- **Regulatory truth** — `get_ingredient_record`, `get_market_status`, `get_notification_requirements`, `get_label_and_claims`, `get_enforcement_watch`
- **Catalogue** — `search_cleared_items`, `get_item`, `compare_items`
- **Supply** — `find_verified_supplier`, `get_availability`, `get_price`
- **Documents** — `get_product_documents`, `verify_gtin`
- **Commerce** — `create_order_intent`, `a2a_handoff`
- **Operations** — `get_safety_label`, `log_audit`

Reads are open. The six transacting tools take a client-credentials Bearer from the issuer, https://a2a-registry.ai (register at `/oauth/register`; see https://a2a-cosmetics.ai/auth.md).

## The agents

<!-- agents:begin -->
| Role | Hostname | Agents | Region |
|---|---|---|---|
| Hub (concierge) | a2a-cosmetics.ai | 1 | South Central US |
| Registry (token questions) | a2a-registry.ai | 1 | South Central US |
| Payments door (receipt questions) | a2a-x402.ai | 1 | South Central US |
| Actor-class entry points | pharmacy · prescriber · wholesaler · brand + .a2a-cosmetics.ai | 4 | South Central US |
| Handoff (referral only) | handoff.a2a-cosmetics.ai | 1 | South Central US |
| Enforcement watch | watch.a2a-cosmetics.ai | 1 | South Central US |
| Auditor Port | audit.a2a-cosmetics.ai | 1 | South Central US |

36 agents live, 0 product-line slots reserved on the tenant table; by region: Australia East 1 · Brazil South 1 · Canada Central 1 · Central India 1 · France Central 2 · Germany West Central 1 · Italy North 1 · Japan East 1 · Korea Central 1 · Mexico Central 1 · Poland Central 1 · South Central US 18 · Southeast Asia 1 · Spain Central 1 · Switzerland North 1 · UAE North 1 · UK South 1 · West Europe 1. Counted from the hub's `list_agents` on 2026-09-20; door version 1.0.0, 20 tools.
<!-- agents:end -->

## Examples

Stock clients, no adapter: `examples/aws_strands.py` · `examples/azure_semantic_kernel.py` · `examples/google_genai.py` · `examples/generic_mcp_client.py`; A2A: `examples/a2a_aws_strands.py` · `examples/a2a_azure_agent_framework.py` · `examples/a2a_google_sdk.py`.

## Pay on it

x402 is live on https://a2a-x402.ai (USDC on Base, settlement after the gate; a refused order intent is not settled and not charged). The Machine Payments Protocol, the Universal Commerce Protocol and the Agentic Commerce Protocol are built and dark until they can settle; the Agent Payments Protocol sits on the Agent Card. Front door: https://a2a-pay.ai.

## Contact

mcp@a2a-cosmetics.ai — the registry contact for every listing this hub files.
