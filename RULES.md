# Scenario C rules

This project tests Cube Core as a semantic layer without business-policy context.

## Required behavior

1. Use only the configured `cube_semantic` MCP for data access. Use `mcp_call` with `server: "cube_semantic"` and tool `cube_metadata` or `cube_query`. If the needed tools are not discoverable or Nao reports them unavailable, use `mcp_connect` with `server: "cube_semantic"` to discover them, then call them through `mcp_call`. Never use Nao's native `execute_sql`, direct PostgreSQL, or another route for data access. If Cube remains unavailable, report the route failure without falling back. `decimal_calculator.calculate` is separately permitted only for arithmetic as specified below; it is not another data-access route.
2. Do not use direct PostgreSQL, SQL against the source database, or another data-access connection.
3. Use only measures and dimensions returned by Cube metadata.
4. Do not invent semantic members or calculate a business metric from memory.
5. Business-policy definitions are unavailable in this condition.
6. If Cube cannot provide a requested definition or result, say so instead of guessing.
7. Report the semantic route and relevant filters/time grain in the answer.
8. For final monetary totals, prefer Cube measures explicitly named with the `Rounded2dp` suffix. They round the aggregate after `SUM`; never round source rows before aggregation. Keep the full-precision measure for ranking, thresholds, and other calculations that depend on exact values. This rule applies only to monetary totals, not counts, quantities, rates, or percentages; do not combine currencies or perform currency conversion unless requested and supported by the model.
9. When asked to produce long lists of results, show the results in one monospace plain-text code block using triple backticks, with one result item on each line. If the complete list won't fit in a single response, provide it as a downloadable text file intsead of leaving entries out

## Required arithmetic: `decimal_calculator.calculate`

For every user-facing result that requires arithmetic over observed or explicitly provided numeric inputs, you **MUST** call the shared MCP `decimal_calculator.calculate` before presenting the calculated result—even when the calculation is simple. This includes averages/division, ratios, differences, percentages, and multi-step or weighted denominators. For an average, retrieve the authoritative numerator and denominator through the scenario's approved data route, then calculate the division with the MCP; do not perform the final arithmetic mentally or substitute a SQL/Cube expression that returns the derived average.

Keep data semantics separate from arithmetic:

- Use only this project's approved data route, specified above, to select, filter, and aggregate source rows and retrieve observed totals/counts. Do not send raw table rows to the calculator for database aggregation.
- Apply only business rules defined by this project's `RULES.md` or a skill explicitly referenced by it before forming the arithmetic expression. The calculator does not retrieve data, select filters, infer missing values, or decide business rules.
- Pass an explicit expression using the exact numeric inputs returned by the data tool, without currency symbols or thousands separators; use only `+`, `-`, `*`, `/`, `^`, and parentheses. Prefer one expression with parentheses for a multi-step result so the trace records the full formula.
- Use the calculator's returned value; do not round intermediate inputs. Round only the final displayed value to the requested precision, and follow any existing source-total rounding rule for source aggregates.
- If the MCP call fails or is unavailable, do not silently fall back to mental/model arithmetic or a SQL/Cube-derived substitute. Preserve the observed inputs, state that the derived result could not be verified, and retry only the identical expression if the failure is transient.

Example: for an ordinary weekday average, pass the actual observed amount and day count as a plain expression such as `12345.67 / 22`. For weighted denominators, encode only the factor and formula explicitly defined for that scenario; never introduce a policy from another scenario.

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
