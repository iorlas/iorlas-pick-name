# iorlas-pick-name

A Claude Code plugin that picks a name for a project, package, repo, channel or skill.

It does not just generate. It reads the neighbouring names first, generates against the gaps, screens every candidate, and checks meaning, pronunciation and distance from its neighbours.

## How it works

1. **Invoke**: reads cheap local context, loads one YAML profile per platform (they stack), echoes what it understood.
2. **Brief**: one free-text dump, your own names first.
3. **Landscape scan**: background agents collect the names already in the area and the habits around them.
4. **Generate and kill**: three persona agents (one is an Outsider who ignores the landscape), hard kills, registry checks by real command.
5. **Cards**: five shuffled candidates plus a labelled wildcard, with evidence. Reply "2 4, worst 3".
6. **Iterate**: each round keeps an explore slot that ignores your stars, so the pool keeps diverging.
7. **Finalist gate**: a challenger and a pre-mortem per finalist, then lock or hold.
8. **Save**: a provisional `names-<slug>.md`; `resume` reopens it.

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
