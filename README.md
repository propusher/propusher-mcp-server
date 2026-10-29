# Propusher MCP

Plugin that connects AI agents to [Propusher](https://propusher.com) via the hosted MCP server at `https://app.propusher.com/mcp`.

## Supported agents

| Agent | Install |
|---|---|
| Claude Code | `claude plugin marketplace add propusher/propusher-mcp-server`<br>`claude plugin install propusher@propusher` |
| Codex / ChatGPT desktop | `codex plugin marketplace add propusher/propusher-mcp-server`, then install Propusher from the Plugins Directory |

You'll be asked to sign in to Propusher on first use.

## Skills

The plugin ships six Agent Skills, loaded when you work with Propusher:

| Skill | Covers |
|---|---|
| [`propusher`](skills/propusher/SKILL.md) | connecting, how the tools fit together, drafts (edits wait for the coach, creates are live, `publish_draft` on request), good practice |
| [`build-exercise`](skills/build-exercise/SKILL.md) | finding, creating and editing exercises; equipment and muscle groups read from the animation |
| [`animate-exercise`](skills/animate-exercise/SKILL.md) | the 3D animation format, and writing and checking an exercise's animation |
| [`build-workout`](skills/build-workout/SKILL.md) | workout templates: prescriptions, per-set plans, classification |
| [`build-program`](skills/build-program/SKILL.md) | multi-week programs: workouts, questionnaires and activities on a day grid |
| [`build-questionnaire`](skills/build-questionnaire/SKILL.md) | check-in, intake and RPE forms: question types and scales |

## Layout

One repository, two manifest formats:

- `plugin.json` + `mcp.json` at the root: the portable [Agent Plugins](https://developers.openai.com/plugins/build/plugins) package read by ChatGPT and Codex (OpenAI listing details under `extensions.com.openai`); `.agents/plugins/marketplace.json` is its marketplace.
- `.claude-plugin/` + `.mcp.json`: the Claude Code plugin and marketplace.
- `skills/` and `assets/` are shared. Keep `version` the same in both `plugin.json` files.
