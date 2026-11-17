# Profile schema

One YAML file per area where a name will live. The skill loads every profile whose `triggers` match the brief.

```yaml
id: pypi                      # file name without .yaml
title: Python package on PyPI
primary: false                # true = decides spoken forms and card layout when several load
triggers: [python package, pypi, pip install, python library]

landscape:                    # step 3: what to scan for neighbours
  - source: PyPI search
    queries: ["<topic keywords>"]
    cap: 30                   # max names to keep from this source
    capture: [name, summary, downloads if shown]

registries:                   # step 5: is the exact name free?
  - where: PyPI
    check: "https://pypi.org/project/<normalised-name>/ returns 404"
    normalise: "lowercase; runs of - _ . collapse to one -"
    on_hit: kill

conventions:                  # habits of this area; shown to personas
  - "py- prefix is common and carries no signal"

constraints:                  # strictest value wins across profiles
  charset: "a-z 0-9 - _ ."
  max_length: 40
  case: lower

spoken_forms:                 # step 6: say it aloud in these frames
  - "pip install <name>"
  - "import <name>"

extra_filters:
  - id: typosquat
    kind: hard                # hard = never shown; soft = shown with a flag
    rule: "Edit distance <= 1 from a top-1000 package"
```

Rules:
- Every field is optional except `id`, `title`, `triggers`.
- Search results are evidence, not proof. A registry check that cannot run is reported as "not checked", never as free.
- Keep a profile to facts about the area. Taste belongs in the brief.
- New area: copy the closest profile, change `id`, `triggers`, and the sources.
