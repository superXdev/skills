# AI Skills

An open-source collection of reusable skills for AI coding agents. Each skill is a focused instruction package that teaches an agent how to handle a repeatable workflow.

## Available skills

| Skill | Purpose |
| --- | --- |
| [`llm-council`](skills/llm-council/SKILL.md) | Use a structured council of language models to examine a decision from multiple perspectives. |
| [`skill-factory`](skills/skill-factory/SKILL.md) | Design, validate, and improve high-quality AI-agent skills. |
| [`bulletproof-react`](skills/bulletproof-react/SKILL.md) | Build, refactor, and review production React applications with scalable architecture and quality practices. Source: [alan2207/bulletproof-react](https://github.com/alan2207/bulletproof-react) (MIT). |

## Install

Use the [Vercel Skills CLI](https://github.com/vercel-labs/skills).

Install all skills:

```bash
npx skills add superXdev/skills
```

List the available skills:

```bash
npx skills add superXdev/skills --list
```

Install one skill:

```bash
npx skills add superXdev/skills --skill llm-council
```

You can also install directly from a local clone while developing:

```bash
npx skills add ./skills
```

## Use

After installation, use your supported AI coding agent normally. Its skill system reads the installed `SKILL.md` instructions when a request matches the skill's description and trigger conditions.

Read a skill's `SKILL.md` before using or changing it to understand its intended workflow, prerequisites, boundaries, and validation steps.

## Contribute

Contributions are welcome.

1. Fork the repository and create a focused branch.
2. Add or update a skill in `skills/<skill-name>/`.
3. Include a `SKILL.md` file with `name` and `description` YAML front matter.
4. Keep related references, templates, and scripts inside that skill directory.
5. Follow the authoring, safety, and validation guidance in [AGENTS.md](AGENTS.md).
6. Verify all referenced files and commands, then open a pull request describing the workflow, triggers, dependencies, safety considerations, and validation performed.

A good skill is focused, actionable, tool-neutral where possible, safe by default, and clear about how to verify its output.

## License

Add a license before publishing or accepting external contributions.
