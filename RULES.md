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

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
