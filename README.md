# iorlas-pick-name

A Claude Code plugin that picks a name for a project, package, repo, channel or skill.

It does not just generate. It reads the neighbouring names first, generates against the gaps, screens every candidate, and checks meaning, pronunciation and distance from its neighbours.

## How it works

1. **Brief**: what is named, audience, languages, vibe, references.
2. **Profiles**: loads one YAML per platform from `skills/pick-name/profiles/`. They stack.
3. **Landscape scan**: collects names already in the area and the habits around them.
4. **Generate**: three personas in parallel, aimed at gaps.
5. **Screen**: hard kills (taken, offensive, typosquat) then soft flags.
6. **Fit and distance**: does it mean something right, is it easy to say, is it too close or too far from neighbours.
7. **Present** 5-7 candidates with evidence; iterate on reactions.

## Profiles

| Profile | For |
|---|---|
| `pypi` | Python package names |
| `github-repo` | GitHub repositories |
| `youtube` | YouTube channels and handles |
| `telegram` | Telegram channels and bots |
| `claude-skill` | Claude Code plugins and skills, including naming conventions |

Add a platform by copying a profile and following `profiles/_schema.md`.

## Install

```sh
claude plugin marketplace add iorlas/iorlas-marketplace
claude plugin install iorlas-pick-name@iorlas-marketplace
```

Then ask: "help me name a Python library for ..." or run `/iorlas-pick-name:pick-name`.

## License

MIT
