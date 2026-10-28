# OpenAPI to Docs

An AI skill that turns an OpenAPI 3.x specification into documentation developers can follow: a five-minute quickstart, an authentication guide, per-endpoint pages with schema-valid examples, an error catalogue, pagination and rate-limit guidance, and changelog conventions. It also lists the gaps in your spec (missing examples, undocumented errors, empty descriptions) so you can fix them at the source.

## Install

Claude Code:

```
git clone https://github.com/AIX-store/skill-openapi-to-docs ~/.claude/skills/openapi-to-docs
```

Other agents (Codex, Cursor, Claude Desktop projects): copy the contents of `SKILL.md` into the agent's instructions or rules file.

## Example prompts

- "Generate developer docs from openapi.yaml, starting with a quickstart."
- "Write an error catalogue and a pagination guide for this API spec."
- "What is missing from this spec before we publish partner docs?"

## Licence

MIT, © 2026 CodeMeld LLC.

Listed on AiX: https://aixstore.ai/store
