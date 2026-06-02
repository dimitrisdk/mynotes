# Model Context Protocol (MCP) — Research Summary

**Date:** 2026-06-02  
**Source:** Research by Panagia (web + Hermes docs)

---

## One-Paragraph Summary

The Model Context Protocol (MCP) is an **open-source, universal standard** introduced by Anthropic (Nov 2024) that defines how AI applications connect to external data sources, tools, and services — acting as a "USB-C port for AI." Instead of writing custom integration code for every combination of AI model × external tool (the N×M problem), MCP provides a single, model-agnostic protocol. An AI agent (client) discovers and invokes capabilities from MCP servers (databases, file systems, APIs) via JSON-RPC 2.0 over stdio (local) or HTTP+SSE (remote). This eliminates fragmented integrations, grounds LLM responses in live data, and enables plug-and-play interoperability.

---

## Key Findings

### 1. The Problem MCP Solves

- **LLM Isolation** — Models can't access live databases, APIs, or local files without custom plumbing
- **N×M Integration Nightmare** — N AI systems × M tools = N×M custom integrations; every new model/tool breaks existing work
- **Fragmented Implementation** — Each vendor has its own function-calling format; no interoperability

MCP transforms N×M → N+M: one standard, one implementation per server, works with any client.

### 2. Architecture

**4 Core Elements:**
1. **Host Application** — Houses the LLM (Claude Desktop, Hermes Agent, VS Code, Cursor)
2. **MCP Client** — Internal component per server, handles protocol + security sandboxing
3. **MCP Server** — Exposes: **Resources** (readable data), **Tools** (executable functions), **Prompts** (templates)
4. **Transport Layer** — STDIO (local subprocess) or HTTP+SSE (remote)

**Protocol Flow:** Connect → Capability Discovery → Tool Registration → User Request → LLM selects tool → JSON-RPC call → Server executes → Result returned → LLM incorporates

### 3. Comparison: MCP vs Traditional Function Calling

| Aspect | MCP | OpenAI Function Calling |
|--------|-----|------------------------|
| Scope | Full protocol (discovery, execution, lifecycle) | Just structured API call translation |
| Standardization | Model-agnostic, open standard | Vendor-specific |
| Communication | Bidirectional, streaming (SSE) | Request-response only |
| Execution | Client-side, anywhere | Server-side logic |
| Portability | Switch models without rewiring | Rewrite schemas per model |

### 4. Ecosystem

**Clients:** Claude Desktop, Claude Code, ChatGPT, VS Code, Cursor, JetBrains, Xcode, LangChain, Hermes Agent

**Popular Servers:** Filesystem, Git, GitHub, Postgres, Slack, Google Drive, Puppeteer, Fetch, Time, Apify, Blender, Stripe, Cloudflare, Linear

### 5. MCP in Hermes Agent

Native MCP client built in — configure in `~/.hermes/config.yaml` under `mcp_servers`. Tools auto-discovered and registered with `mcp_{server}_{tool}` naming. Supports STDIO (npx/uvx) and HTTP transports, sampling (server-initiated LLM requests), credential-safe environment filtering, auto-reconnect.

### 6. Creators

David Soria Parra & Justin Spahr-Summers at Anthropic. Open-source (MIT/Apache 2.0). SDKs in Python, TS, Java, Kotlin.

---

## Sources

- https://www.anthropic.com/news/model-context-protocol
- https://modelcontextprotocol.io/
- https://github.com/modelcontextprotocol
- https://www.descope.com/learn/post/mcp
- https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp