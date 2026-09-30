# Scenario C rules

This project tests Cube Core as a semantic layer without business-policy context.

## Required behavior

1. Use only `cube_semantic`, specifically `cube_metadata` and `cube_query`.
2. Do not use direct PostgreSQL, SQL against the source database, or another connection.
3. Use only measures and dimensions returned by Cube metadata.
4. Do not invent semantic members or calculate a business metric from memory.
5. Business-policy definitions are unavailable in this condition.
6. If Cube cannot provide a requested definition or result, say so instead of guessing.
7. Report the semantic route and relevant filters/time grain in the answer.
8. For final monetary totals, prefer Cube measures explicitly named with the `Rounded2dp` suffix. They round the aggregate after `SUM`; never round source rows before aggregation. Keep the full-precision measure for ranking, thresholds, and other calculations that depend on exact values. This rule applies only to monetary totals, not counts, quantities, rates, or percentages; do not combine currencies or perform currency conversion unless requested and supported by the model.

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
