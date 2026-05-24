# Stack Trace

An interactive demo for the Czechitas data-analysis workshop. Click a scenario, watch a single SQL request flow through all five agentic layers (Prompt → Config → Skill → MCP → Evals), then download real config files to drop into your own GitHub Copilot or OpenCode project.

![Screenshot](screenshot.png)

**Live demo:** https://ishalkin.github.io/czechitas-stack-trace/

**Sibling tool:** https://ishalkin.github.io/czechitas-skill-builder/ — coloring book for the SKILL.md file.

## What it does

Three preset scenarios show the same agentic stack from three different angles:

1. **Top 10 zemí** — query succeeds, but evals catch a subtle data-quality issue (`COUNTRY_DIRTYDATA` 408 ≠ 204)
2. **Zkontroluj SQL** — the canonical broken query (`TEROR2_OLD`, `YEAR`, `victims`) is intercepted by the skill before MCP is even called
3. **Smaž TEROR2_OLD** — `DROP TABLE` blocked by the `refuse destructive` rule

Toggles in the left rail let participants turn off individual rules, MCP servers, or evals — and watch the bad outcomes the slides warned about reappear.

Click any layer chip to expand "pod kapotou" and see the actual artifacts: AGENTS.md preamble, SKILL.md rules, JSON-RPC tool calls, eval assertions.

## Why

Slide 3 of the workshop deck shows the five layers as a static table. This demo makes them collaborate live on one query, so the connection rule-file → agent-behavior → eval-verdict becomes visible — not abstract.

## Local development

No build step. No dependencies. Just open the file:

```bash
python -m http.server 8000
# → http://localhost:8000
```

To run the embedded self-tests:

```
http://localhost:8000/?test=1
```

A successful run shows "ALL TESTS PASSED" in navy.

## Architecture

Single `index.html`. Pure simulation: a deterministic `run(scenario, state)` function returns a 5-layer trace and final output. Toggling state re-runs the function and re-renders. No network, no LLM, no Snowflake.

## License

MIT.
