# Research: test query

**Date:** 2026-06-05
**Task ID:** t_e03c3bd6
**Assignee:** Solomon

## Query
test

## Findings

### Hermes Agent Overview (Official Documentation)
- **Source:** [Hermes Agent Official Website](https://hermes-agent.nousresearch.com/) - The self-improving AI agent built by Nous Research with built-in learning loop
- **Source:** [Hermes Agent Documentation](https://hermes-agent.nousresearch.com/docs/) - Complete documentation with installation, features, and user guides
- **Source:** [GitHub Repository](https://github.com/nousresearch/hermes-agent) - Open source repository with source code

### Key Features (from official docs)
- **Closed Learning Loop:** Agent-curated memory, autonomous skill creation, skill self-improvement during use, FTS5 cross-session recall, Honcho dialectic user modeling
- **Runs Anywhere (6 Terminal Backends):** Local, Docker, SSH, Daytona (serverless), Modal (serverless), Singularity (HPC)
- **Lives Where You Do (20+ Platforms):** Telegram, Discord, Slack, WhatsApp, Signal, Matrix, Mattermost, Email, SMS, DingTalk, Feishu, WeCom, Weixin, QQ Bot, Yuanbao, BlueBubbles, Home Assistant, Microsoft Teams, Google Chat
- **Model Flexibility:** Works with Nous Portal, OpenRouter, OpenAI, or any custom endpoint
- **Scheduled Automations:** Built-in cron with delivery to any platform
- **Delegation & Parallelization:** Spawn isolated subagents, programmatic tool calling via execute_code
- **Open Standard Skills:** Compatible with agentskills.io, portable/shareable community-contributed via Skills Hub
- **Full Web Control:** Via Nous Portal - search, extract, browse, vision, image generation, TTS
- **MCP Support:** Connect to any MCP server
- **Research-Ready:** Batch processing, trajectory export, RL training with Atropos

### Installation Options
- **Windows/macOS:** Download Hermes Desktop installer from official site
- **Linux/macOS/WSL2/Android:** `curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash`
- **Windows (PowerShell):** `iex (irm https://hermes-agent.nousresearch.com/install.ps1)`
- **Fastest path:** Run `hermes setup --portal` after install for one OAuth covering model + 4 Tool Gateway tools

## Summary
This was a test research task (t_e03c3bd6) to verify the kanban workflow for Solomon research tasks. The official Hermes Agent documentation confirms it's a self-improving autonomous AI agent by Nous Research with a closed learning loop, cross-platform deployment, multi-platform messaging, flexible model support, and extensive automation/delegation capabilities.

## Links
- [Hermes Agent Official Website](https://hermes-agent.nousresearch.com/)
- [Hermes Agent Documentation](https://hermes-agent.nousresearch.com/docs/)
- [GitHub - NousResearch/hermes-agent](https://github.com/nousresearch/hermes-agent)
- [Nous Portal](https://hermes-agent.nousresearch.com/docs/integrations/nous-portal)
- [agentskills.io](https://agentskills.io/)