# Scenario C — Cube Core semantic layer without business skill

This repository is the Nao Multi-project context for formal scenario **C**.

## Boundary

- No native PostgreSQL database is declared.
- Cube Core is accessed only through the `cube_semantic` MCP.
- Available MCP tools are `cube_metadata` and `cube_query`.
- The agent uses the measures, dimensions, filters, and time grains exposed by Cube.
- No Contoso business skill or domain-specific business-policy documentation is available.
- `direct_postgres` must not be present or usable in this project.

## Project mapping

- Nao project: `tesis-condition-c`
- Repository: `cfocoder/cube_nao_semantic_layer_c`
- Expected route: `cube_semantic` → Cube REST → Cube Core → PostgreSQL
