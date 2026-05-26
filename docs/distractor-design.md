# Distractor Design

Six options per MCQ card: **1 correct + 5 distractors**. The five distractors are chosen by a fixed recipe to mix flavor and pressure.

## The recipe (per card, per direction)

| Slot | Count | What | Why |
|------|------:|------|-----|
| 1–2  | 2 | Near-semantic | Forces precision. The card is *not* trivially solved by "the only word in the topic area." |
| 3–4  | 2 | Same frequency-decile filler | Controls for users guessing the rarest-looking option. All six options come from similar rank tiers. |
| 5    | 1 | Form-confusable | Tests precision on similar shapes: Levenshtein-near for Latin scripts, shared-character neighbor for CJK. |

Same recipe is applied symmetrically for **recognition** (5 wrong target-language *words* + the correct word) and **recall** (5 wrong English *glosses* + the correct gloss).

## How "near-semantic" is chosen

For each card target word *t* we want the candidate *c* maximizing cosine similarity of `embed("{t} — {gloss(t)}")` vs `embed("{c} — {gloss(c)}")`.

- **Model:** BAAI/bge-m3 (1024-dim multilingual)
- **Pool:** every word in the language's top-N frequency list (50K typically)
- **Filter:** same coarse POS, same frequency decile (so a noun target draws noun distractors of similar everyday-ness)
- **Tiebreaker:** prefer pool words with *lower* current usage count (rotation)

If the target has no embedding (rare — proper nouns or junk), the selector falls back to Jaccard on English-gloss content tokens.

### Why BGE-M3 specifically

We A/B'd BGE-M3 (1024-dim, multilingual) against a smaller `paraphrase-multilingual-MiniLM-L12-v2` (384-dim) on the same pool. BGE-M3 produced visibly stronger semantic neighborhoods, e.g.:

| Target | Top-5 neighbors |
|--------|-----------------|
| `domingo` (Sunday, ES) | `sábado, lunes, viernes, semana, verano` |
| `feliz` (happy, ES) | `felicidad, alegre, contento, felicidades, alegría` |
| `司机` (driver, ZH) | `驾驶员, 卡车司机, 開车, 汽车` |
| `救い出す` (rescue, JA) | `運び出す, 掘り出す` — **all share `〜出す` verb pattern** |

For the case where BGE-M3 vs Jaccard differ: ~95% of cards across all four languages.

## Rotation fairness

Without rotation, frequency-decile filtering naturally favors the same few "central" words. They'd appear as distractors hundreds of times each while half the pool never gets used. That's bad: users learn "X is never the answer" and the MCQ collapses.

**Mechanism:**
1. Maintain a `usage_count[word]` counter across all picks in the deck build.
2. Hard cap: never pick a candidate with `usage_count[word] ≥ 8`.
3. Among eligible candidates, the composite score is `cosine × 100 - usage_count + small_random_jitter`. The cosine term dominates within-decile, but usage acts as a hard cap and a soft tiebreaker.

**Observed distribution** (recognition direction, on a 10K-card / 10K-pool deck):

```
total unique words used as distractors: 10,000  (100% of pool)
max uses by any single word:                8   (= cap)
mean uses per pool word:                  ~5.0
histogram:
   4 uses: 4,026 words
   5 uses: 3,294 words
   6 uses: 1,602 words
   7 uses:   810 words
   8 uses:   268 words
```

So the *most-used* distractor word appears in 8 different cards (0.08% of the deck), and 80% of the pool appears 4–6 times. Roughly uniform with a long upper tail.

## Form-confusable selection

For Latin-script languages: Levenshtein distance ≤ 2 vs the target. So `policía` pulls `podía`, `comer` pulls `creer`, `entonces` pulls `entiendes`.

For CJK / Hangul: shared first character or shared length. So `孩子` pulls `小子`, `明日` pulls `昨日`, `이렇게` pulls `그렇게`.

This slot deliberately picks words a learner is *likely to confuse on sight*. It's pedagogically the most valuable distractor for "I think I know this word" moments.

## Disjointness rules

- Recognition: distractor word ≠ target word.
- Recall: distractor gloss ≠ target gloss AND distractor word ≠ target word (avoids near-identical inflections of the target appearing as a distractor with a slightly different English gloss).
- Both: 6 options shuffled per card with a deterministic seed (so the JSON output and `.apkg` always match across runs).
