# Scenario C deployment contract

## Required Nao project

```text
Project: tesis-condition-c
Repository: https://github.com/cfocoder/cube_nao_semantic_layer_c.git
Branch: main
```

## Required runtime variables

```text
CUBE_API_URL
CUBE_API_SECRET
OPENAI_API_KEY / OPENAI_BASE_URL
OPENROUTER_API_KEY / OPENROUTER_BASE_URL
```

The existing deployment bootstrap must provide `/app/context/agent/mcps/cube_server.mjs` and the MCP SDK. The repository supplies `agent/mcps/mcp.json`; it does not contain credentials or the generated adapter source.

Do not add `direct_postgres` to this project's `nao_config.yaml`, environment, or deployment override.
