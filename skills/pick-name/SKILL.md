---
description: Pick a name for a project, package, repo, channel, product or skill. Use for "I need a name for...", "help me name...", "screen these names", or "resume" on a saved shortlist. Scans neighbouring names first, generates with personas, screens every candidate, loads one profile per platform.
---

# Pick a name

Goal: one name the user is happy with, with evidence it is free, means something fitting, and is easy to say.

The user is busy and expert. Keep their effort to short replies. Do the work silently, show the result.

## Flow

| # | Stage | User does | Skill does |
|---|---|---|---|
| 1 | Invoke | types the intent | infers the brief, loads profiles, echoes one line |
| 2 | Brief | one free-text dump | fills the rest by inference, states the guesses |
| 3 | Scan | nothing | background landscape scan, tells how long |
| 4 | Generate and kill | nothing | persona agents, hard kills, registry checks, one status line |
| 5 | Cards | reacts in one line | shuffled cards plus one wildcard |
| 6 | Iterate | stars, kills, continues | next round with an explore slot |
| 7 | Finalist gate | "lock X" or "hold" | challenger and pre-mortem per finalist |
| 8 | Save | nothing | writes the shortlist file, marks it provisional |

Screen-only (user brings candidates): skip 3 and the generation part of 4; their names go through the same kills and cards.

## 1. Invoke

Read cheap local context only to infer the brief: the directory name, the README's first lines, a package manifest. Do not read further.

1. List `profiles/*.yaml` in `${CLAUDE_SKILL_DIR}`; read each `id` and `triggers`; read `profiles/_schema.md` once.
2. Load every profile whose triggers match. Several load at once. No match: run on universal filters only and say so; offer a new profile from the schema.
3. Merge: `constraints` strictest wins; `registries`, `landscape`, `extra_filters` union; conflicting `conventions` shown to the user, not silently resolved. The `primary: true` profile (else the first) sets spoken-form tests.
4. Print one line:

   `Naming <what>, loading <profiles>. I understood: <brief in one line>. Enter = ok, or correct me.`

If the user said `resume` or points at a saved shortlist, read the file, show its state in two lines, and jump to stage 5 or 7.

## 2. Brief

One message, not an interview:

`Type what it is, who it's for, the vibe. Also: names you already have, and names you hate.`

- Anything already answered by the invocation is not asked again.
- Infer language, platforms, domain; state each guess in the echo so one word corrects it.
- **Own names first.** Names the user types go into the pool and pass the same filters as every generated name. They appear on cards like any other; mention afterwards which card was theirs.
- Anti-references become kill history at once.
- Ask a follow-up only when generation would otherwise be blind (no idea what the thing does).

## 3. Landscape scan

Say first: `Scan runs about 3 minutes in the background.` Then start one agent per `landscape` source with `run_in_background`, in one message. While they run, ask once whether to add anything to the brief, then stay quiet.

Each agent returns names found (cap per source, with one-line purpose), recurring patterns, crowded zones, gaps. Merge into a 3-line map: **crowded / gaps / habits**. Keep the raw list for kills.

## 4. Generate and kill

Spawn 3 persona agents with the Agent tool in one message, 8-12 candidates each. Never write the candidates yourself in place of the agents. Persona text, the Outsider and the selection rules are in `personas.md`.

Round 1: two personas get the brief and the landscape map; one is **the Outsider**, who gets only what the thing is, no vibe and no landscape.
Round 2+: two personas work near the starred names; one is the **explore slot** and ignores the stars on purpose. Say so on the cards ("explore:").

Prompt per persona:

```
BRIEF: ...
PROFILE CONSTRAINTS: (merged constraints and conventions)
LANDSCAPE: (map; omit for the Outsider)
KILL HISTORY: ...
STARRED: ...   (omit for the explore slot)

Generate 8-12 candidates. For each: name; short form; what it means or why;
test phrases from each loaded profile's spoken_forms.
```

Then, without asking:
1. Hard kills from `filters.md`, then every profile `extra_filters` with `kind: hard`, then collision against the scanned names.
2. Registry checks: run each registry's `cmd` with Bash (`gh api`, `curl`). Web search finds near neighbours and meaning clashes; it never proves a name is free. A `cmd` that cannot run is "not checked" on the card.
3. Print one status line: `31 generated, 19 killed, 12 alive. k = kill log.`

## 5. Cards

- Pick 5 survivors at random. **Never sort by flags**: the safest name on top becomes the anchor. Add **one wildcard**, labelled: `Wildcard (off-brief on purpose): <name>. Pick it only if it grows on you.`
- Mix the user's own names in. Hold the rest in reserve.
- Fewer than 3 survive: say so and offer another round.

```
### 2. <name>
Means: <1-2 sentences>
Sound: <short form, syllables, spoken test result>
Nearest neighbour: <name> (close | ok | far)
Free on: <registry: result>   (not checked: <registry>)
⚠️ <flag>: <detail>
```

Fit and distance per card: **meaning** (what a cold reader thinks it does), **sound** (say it in each `spoken_forms` frame), **distance** (too close = confusion or typosquat; too far = no meaning), soft flags from `filters.md` and profiles. When heuristics and the cold impression disagree, the impression wins.

Ask: `Keep? Worst? e.g. "2 4, worst 3". ⭐ marks a favourite.` Reading the worst pick shows the user's real criteria faster than rating all cards. Do not ask "why" on every kill; ask once, in round 2, if the pattern is unclear.

## 6. Iterate

1. Show reserves first, then generate.
2. Update the brief with what survived and what died.
3. Rotate a persona whose whole output died.
4. Never regenerate a killed name or close variant. From round 5, compress kill history into pattern clusters and keep the 10 latest kills.
5. Three rounds with no star: stop, show the user's keep and worst picks side by side, and revisit the brief.

## 7. Finalist gate

With 1-3 names left, before the user locks one:

- **Challenger**: `Before you lock <X>: the best alternative that differs on one axis: <Y>. Enter to skip.`
- **Pre-mortem**, written by the skill, one line per finalist: `A year on, you regret <X> because: <specific failure>.` Draw from real flags: typo of a neighbour, unsearchable, outgrown by the product's next version.

The user answers `lock <name>` or `hold`.

## 8. Save

Write `names-<project-slug>.md` in the current directory: brief, profiles, shortlist cards, kill log, reactions. Say: `Provisional. Saved to names-<slug>.md. Sleep on it; "resume" reopens.` Suggest next steps (register handles, check domains). The user moves the file wherever they keep notes.

## Notes

- Search results are evidence, not proof: report "not found" as "no hit in this search".
- Conventions for naming and building skills live in `profiles/claude-skill.yaml`; this skill follows them.
- Wildcard is a trial: if the saved files show it is never picked, replace it with a challenger shown only at stage 7.
