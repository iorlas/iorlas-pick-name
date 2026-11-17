# Universal filters

Platform-specific checks (handle taken, creator collision, registry normalisation) live in the profiles. These apply to every name.

## Hard kills (name never shown; log the reason)

| # | Filter | Method |
|---|---|---|
| 1 | Word collision | Short form is a common word in the target language with a dominant meaning (grandpa, worst grade, ban). |
| 2 | Gender coding | Name or short form reads as the wrong gender for the creator, when that matters. |
| 3 | Cultural or slang trap | Prison, bandit, sexual, scatological, insult, drug associations in the target culture. |
| 4 | Cross-language trap | Offensive or ridiculous in any language the audience speaks. English always included. |
| 5 | Similar-sound confusion | Reads as a typo of a well-known name. |
| 6 | Truncation quality | Unpronounceable cluster; short form does not sound like a name. |

Existing-owner collision (a creator, package or repo already holds the name) is a hard kill that each profile runs through its own `registries`.

## Soft flags (shown with ⚠️)

| # | Filter | Method |
|---|---|---|
| 7 | Phonetic science | Breaks recall or fluency heuristics (below). Mild or moderate. |
| 8 | Fiction collision | Search: dominated by a character, movie, game. Name which. |
| 9 | Brand collision | Search: known brand in another domain. Name which. |
| 10 | Ego or self-declaring | Name claims status or quality for its creator. |
| 11 | Discoverability | Search: page one dominated by something a newcomer cannot outrank. Low, medium or high. |
| 12 | Telephone test | Heard aloud, can it be spelled? Ambiguous spelling or transliteration. |
| 13 | Derivative potential | Flag only when the root blocks natural extensions or makes awkward compounds. Otherwise report neutral in the card. |
| 14 | Cold impression | As a cold reader with zero context, what does it feel like? Flag flat, dead, unpleasant or wrong signal. |
| 15 | Five-year decay | Tied to a current meme or trend. |

## Conflict rule

Phonetic heuristics (7) lose to the cold impression (14). The feeling is the outcome the heuristics try to predict.

## Phonetic reference

Guidelines, not laws. Great names break any of these.

| Rule | Evidence |
|---|---|
| Plosive start (k p t b d g) gives the best recall | phoneme analyses of popular brands |
| 1-2 syllables, 4-8 characters recall best | working-memory research |
| Processing fluency raises trust | fluency literature |
| Rounded sounds (b m l o u) read warm; sharp sounds (k t i e) read precise | bouba-kiki effect, replicated across many languages |
| Arbitrary names beat descriptive ones long term | brand case studies |
| Across scripts, prefer shared phonemes | cross-script brand studies |
