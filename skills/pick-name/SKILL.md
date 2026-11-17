---
description: Pick a name for a project, package, repo, channel, product or skill. Use for "I need a name for...", "help me name...", "screen these names". Scans neighbouring names first, generates with personas, screens every candidate, loads one profile per platform.
---

# Pick a name

Goal: one name the user is happy with, with evidence it is free, means something fitting, and is easy to say.

Two entry modes:
- **Full**: brief → profiles → landscape → generate → screen → present.
- **Screen-only**: user brings candidates; skip generation and landscape fill-in, run profiles and screening.

## Ladder

| # | Step | Output |
|---|---|---|
| 1 | Brief | what is named, audience, languages, vibe, references, anti-references |
| 2 | Pick profiles | list of `profiles/*.yaml` to load |
| 3 | Landscape scan | map of taken and crowded names plus naming habits |
| 4 | Generate | 24-36 candidates from 3 personas, aimed at gaps in the map |
| 5 | Hard kills | survivors; kill log |
| 6 | Fit and distance | scored survivors |
| 7 | Present | 5-7 cards; user reacts; iterate |

## 1. Brief

Conversation, one question at a time. Stop when generation can start.

Required: what is named · audience and language(s) · vibe (spectrums, not binaries) · domain.
Ask when relevant: must-include sounds · syllable rules · reference names (what it should FEEL like) · anti-references · platforms · languages to cross-check · kill history.

The brief is a living document: update it after each round with the user's reactions.

## 2. Pick profiles

A profile is a YAML file in `${CLAUDE_SKILL_DIR}/profiles/`. Read `profiles/_schema.md` once.

1. List `profiles/*.yaml`; read each file's `id` and `triggers`.
2. Load every profile whose triggers match the brief. Several load at once (a Python skill may load `pypi`, `claude-skill`, `youtube`).
3. Say which profiles loaded and why, in one line. Let the user add or drop one.
4. Merge rules: `constraints` take the strictest value; `registries`, `landscape`, `extra_filters` are unioned; conflicting `conventions` are shown to the user, not silently resolved.
5. No profile matches → run on the universal filters only and say so. Offer to write a new profile from `_schema.md`.

The profile marked `primary: true` (or the first listed) decides spoken-form tests and presentation.

## 3. Landscape scan

Before generating, learn the neighbourhood. For each loaded profile run its `landscape` sources, one agent per source type in parallel. Each returns:
- names found (cap per source in the profile) with one-line what-it-does
- recurring patterns (prefixes, suffixes, metaphors, casing)
- crowded zones (so many near-duplicates that a new name drowns)
- gaps (sounds, meanings, structures nobody uses)

Output a short map. Feed it to the personas as `LANDSCAPE`. Keep the raw name list for step 5.

## 4. Generate

3 personas in parallel, 8-12 candidates each. Persona text and selection rules: `personas.md`.
Prompt template per persona:

```
BRIEF: ...
PROFILE CONSTRAINTS: (merged constraints and conventions)
LANDSCAPE: (map from step 3: crowded zones to avoid, gaps to aim at)
KILL HISTORY: ...
REACTION PATTERNS: ...
STARRED: ...

Generate 8-12 candidates. For each: name; short or nickname form; what it means
or why chosen; test phrases from each loaded profile's `spoken_forms`.
```

From round 5 on, compress kill history into pattern clusters; keep the 10 latest kills verbatim.

## 5. Hard kills

Run `filters.md` universal hard kills, then every loaded profile's `extra_filters` marked `kind: hard`, then exact and near collision against the scanned names and `registries`. A killed name is never shown; log the reason. Kill log on request.

## 6. Fit and distance

- **Meaning**: what a cold reader thinks it does. Does it point at the thing, or somewhere wrong?
- **Sound**: say it aloud in each `spoken_forms` sentence. Spelling ambiguity, awkward clusters, mishearing.
- **Distance to neighbours**: too close = confusion or typosquat risk; too far = carries no meaning. Report close / ok / far with the nearest neighbour named.
- Soft flags from `filters.md` and profiles (`kind: soft`).

When phonetic heuristics conflict with the cold emotional impression, the impression wins.

## 7. Present

Max 5-7 survivors, fewest and mildest flags first; hold the rest in reserve and show them if the top tier dies. Fewer than 3 survive: say so and offer another round.

```
### <name>
Means: <what it says, 1-2 sentences>
Sound: <short form, syllables, spoken test result>
Nearest neighbour: <name> (<close|ok|far>)
Free on: <registries checked and result>
⚠️ <flag>: <detail>
```

Ask for quick reactions: 🔥 keep · ❄️ kill (ask why once) · ⭐ closer. Starred names steer the next round.

## Iterating

1. Show reserves first, then generate.
2. Update the brief with surviving and dying patterns.
3. Swap personas whose output died entirely; re-run from step 4.
4. Never regenerate a killed name or close variant.
5. Three rounds with no star: stop and revisit the brief, the gap may be there.

## Ending

With 1-3 finalists: show final cards, suggest next steps (register handles, check domains, sleep on it). The skill keeps no state between sessions; the user saves the shortlist.

## Notes

- Web searches make a full round slow. Say so up front.
- Search results are evidence, not proof: report "not found" as "no hit in this search".
- Conventions for naming and building skills live in `profiles/claude-skill.yaml`; this skill follows them.
