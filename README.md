# ScreenlyyID agent skills

Skills that let an AI coding agent (Claude Code, Cursor, GitHub Copilot, ChatGPT and others) add [ScreenlyyID](https://www.screenlyyid.com) identity verification to an app without help. ScreenlyyID (screenlyyid.com) is an identity verification, KYC and AML API.

| Skill | What it does |
|---|---|
| [`integrate-screenlyyid`](integrate-screenlyyid/SKILL.md) | General entry point: picks the right guide, detects the app's stack, then adds the server routes, the hosted capture page and the result handling. |
| [`screenlyyid-idv-mobile`](screenlyyid-idv-mobile/SKILL.md) | Identity verification in an iOS, Android, React Native or Flutter app (with native iOS and Android examples). |
| [`screenlyyid-idv-web`](screenlyyid-idv-web/SKILL.md) | Identity verification in a web app. |
| [`screenlyyid-bin-lookup`](screenlyyid-bin-lookup/SKILL.md) | BIN and card checks at checkout. |
| [`screenlyyid-phone-validation`](screenlyyid-phone-validation/SKILL.md) | Phone validation. |
| [`screenlyyid-email-validation`](screenlyyid-email-validation/SKILL.md) | Email validation and verification. |
| [`screenlyyid-address-verification`](screenlyyid-address-verification/SKILL.md) | Address verification. |
| [`screenlyyid-device-ip`](screenlyyid-device-ip/SKILL.md) | Device and IP intelligence. |
| [`screenlyyid-eidv`](screenlyyid-eidv/SKILL.md) | Electronic identity verification (1x1 and 2x2). |
| [`screenlyyid-aml-screening`](screenlyyid-aml-screening/SKILL.md) | AML screening of people and entities. |
| [`screenlyyid-stacking-checks`](screenlyyid-stacking-checks/SKILL.md) | Several checks for one customer under one correlation id. |

The skill points at the documentation instead of copying it, so it stays correct as the docs change:

- Docs for agents: https://www.screenlyyid.com/docs/ai-quickstart/
- All docs as one Markdown file: https://www.screenlyyid.com/docs/llms-full.txt
- Examples: https://www.screenlyyid.com/docs/recipes/
- Install instructions: https://www.screenlyyid.com/docs/agent-skills/

## Install

**Claude Code.** Copy the skill folder into your project (shared with your team) or into your user skills:

```bash
git clone https://github.com/ScreenlyyID/agent-skills.git
mkdir -p .claude/skills && cp -r agent-skills/screenlyyid-* agent-skills/integrate-screenlyyid .claude/skills/        # this project
# or: mkdir -p ~/.claude/skills && cp -r agent-skills/screenlyyid-* agent-skills/integrate-screenlyyid ~/.claude/skills/   # every project
```

Then ask: "Add ScreenlyyID identity verification to this app."

**Other agents.** Most agents read an instructions file. Put the contents of [`SKILL.md`](integrate-screenlyyid/SKILL.md) (without the first `---` block) where yours looks:

- Cursor: `.cursor/rules/integrate-screenlyyid.mdc`
- GitHub Copilot: `.github/copilot-instructions.md`
- Agents that read `AGENTS.md`: add it there.
- ChatGPT or any chat: paste it with your request.

## What you still do yourself

Create a **test** API key: register at https://admin.screenlyyid.com/register, open **Integrations** in the dashboard and create a key in the test environment (it starts with `test_`). An agent cannot sign up for you. Never give an agent a production key.

## Licence

[MIT](LICENSE)
