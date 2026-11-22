# Zoltraak

Zoltraak is a collection of small, practical AI skills that do one thing well.

Named after the basic spell practiced to speed and precision in *Frieren*: getting common tasks right on the first try matters more than elaborate workflows.

## Skills

| Skill | Purpose |
| --- | --- |
| [`execplan`](skills/execplan/SKILL.md) | Maintain one lightweight Markdown plan as a shared blackboard for cross-session resumption and multi-agent/stateless delegation. |

## Usage

Each skill is a self-contained directory under `skills/<name>/SKILL.md` with no vendor-specific hooks or tool lock-in. Copy or link any skill folder into your agent's skills directory (e.g. `~/.agents/skills/`), or install directly from this repository with your skill manager.

## Principles

- **Real, recurring work**: Every skill targets a repeatable failure mode seen in real usage.
- **Observable improvement**: Verified against a no-skill baseline before inclusion.
- **Minimal and portable**: Pure Markdown where possible, cross-agent by default, and no filler steps.

## License

[MIT](LICENSE)
