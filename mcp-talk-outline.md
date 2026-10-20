# MCP (Model Context Protocol) — Tech Talk Outline

Working outline for the internal tech talk. The full sourced research trail lives in this repository (`mcp-research-digest`): one issue per topic, with sourced bullet points in each issue's comments.

> Note: this file was intended for a gist (`mcp-talk-outline.md`), but the available API token has no gist scope. It is kept here on the `outline/draft` branch in the meantime.

## 1. Why MCP exists — the N×M integration problem
- Before MCP: every AI app × every data source needed its own bespoke connector.
- MCP: a single open protocol for connecting AI applications to external data, tools, and systems.
- Introduced and open-sourced by Anthropic in November 2024.

## 2. Core architecture
- Roles: **Host** (the AI application, e.g. Claude Desktop) instantiates **Clients**; each client holds a 1:1 connection to exactly one **Server** and routes messages between host and server (isolated security/permission boundaries).
- Wire format: **JSON-RPC 2.0**; lifecycle = initialize → capability negotiation → operate → shutdown.
- Server primitives: **Tools** (executable actions — `tools/list`, `tools/call`), **Resources** (read-only context — `resources/list`, `resources/read`), **Prompts** (reusable templates — `prompts/list`, `prompts/get`).
- Client capabilities: sampling, roots, elicitation.
- Design inspiration: the Language Server Protocol (LSP).

## 3. Transports & messaging
- Two first-class transports: **stdio** (local, same machine) and **Streamable HTTP** (remote; one HTTP POST per JSON-RPC message, SSE for server→client).
- Legacy HTTP+SSE transport is deprecated (year-long offramp in the 2026-07-28 spec).
- 2026-07-28 stateless rework: no handshake, no session IDs; `Mcp-Method` / `Mcp-Name` headers enable gateway routing.

## 4. Ecosystem & adoption
- **OpenAI** — Agents SDK (March 2025), then ChatGPT desktop and Responses API.
- **Google DeepMind** — Gemini models and SDK (April 2025).
- **Microsoft** — Copilot Studio (GA) and Microsoft 365 Copilot declarative agents.
- Others: Block, Replit, Sourcegraph, Cursor, VS Code; prebuilt servers for Google Drive, Slack, GitHub, Git, Postgres; 10,000+ community servers.
- Governance: donated to the **Agentic AI Foundation** (Linux Foundation) in December 2025.

## 5. Where the spec is heading
- Latest revision: **2026-07-28** — stateless protocol core, Multi Round-Trip Requests, header-based routing, cacheable list results, formal extensions framework (Tasks, MCP Apps, EMA), authorization hardening, and deprecations (Roots, Sampling, Logging, legacy HTTP+SSE).
- Takeaway: MCP is now a first-class HTTP workload that scales on commodity infrastructure.

## Sources
- Introducing the Model Context Protocol — Anthropic: https://www.anthropic.com/news/model-context-protocol
- Model Context Protocol — Wikipedia (creators, adoption, AAIF donation): https://en.wikipedia.org/wiki/Model_Context_Protocol
- Architecture overview — MCP docs: https://modelcontextprotocol.io/docs/learn/architecture
- Specification (2025-06-18) — MCP: https://modelcontextprotocol.io/specification/2025-06-18
- Transports (2025-06-18) — MCP spec: https://modelcontextprotocol.io/specification/2025-06-18/basic/transports
- The 2026-07-28 Specification — MCP blog: https://blog.modelcontextprotocol.io/posts/2026-07-28/
- Key Changes (2026-07-28 changelog) — MCP: https://modelcontextprotocol.io/specification/2026-07-28/changelog
- Releases — modelcontextprotocol/modelcontextprotocol (GitHub): https://github.com/modelcontextprotocol/modelcontextprotocol/releases
- OpenAI adopts rival Anthropic's standard for connecting AI models to data — TechCrunch: https://techcrunch.com/2025/03/26/openai-adopts-rival-anthropics-standard-for-connecting-ai-models-to-data/
- Google to embrace Anthropic's standard for connecting AI models to data — TechCrunch: https://techcrunch.com/2025/04/09/google-says-itll-embrace-anthropics-standard-for-connecting-ai-models-to-data/
- Model Context Protocol (MCP) is now generally available in Microsoft Copilot Studio — Microsoft Copilot Blog: https://www.microsoft.com/en-us/copilot/blog/copilot-studio/model-context-protocol-mcp-is-now-generally-available-in-microsoft-copilot-studio/
- Build declarative agents for Microsoft 365 Copilot with MCP — Microsoft 365 Dev Blog: https://devblogs.microsoft.com/microsoft365dev/build-declarative-agents-for-microsoft-365-copilot-with-mcp/
- MCP Transport Comparison: stdio vs SSE vs Streamable HTTP: https://gingerlabs.ai/blog/mcp-transport-comparison
- A Year of MCP: From Internal Experiment to Industry Standard — Pento: https://www.pento.ai/blog/a-year-of-mcp-2025-review
