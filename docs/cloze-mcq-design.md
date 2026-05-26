# Cloze-MCQ card format

This document explains the card layout shared by all 5 cloze-MCQ deck types:

- **Function-Word Cloze**
- **Grammar-Pattern Cloze**
- **Sentence-Mining Cloze**
- **Conjugation Drill**
- **Counter / Measure-Word**
- **Honorifics / Register**

(The Synonyms deck uses a similar 4-option MCQ but without a sentence-blank — the prompt is the target word + gloss directly.)

---

## Visual layout

The Imperial Golden Age theme: gold accents (`#d4af37`, `#c9b037`) on a dark navy background (`#1a1a2e`), Georgia serif throughout, decorative double border. Identical across all languages and deck types.

### Front

```
╔═════════════════════════════════════════════════════════╗
║              [ LANGUAGE BADGE ]                         ║
║                                                         ║
║              T A R G E T   I T E M                      ║
║              <role or grammar pattern>                  ║
║                                                         ║
║         — C L O Z E   S E N T E N C E —                 ║
║                                                         ║
║       Sentence with ____ in place of the answer.        ║
║       en:  English translation                          ║
║                                                         ║
║       Which fits?                                       ║
║                                                         ║
║       A.  distractor                                    ║
║       B.  correct answer (position randomized)          ║
║       C.  distractor                                    ║
║       D.  distractor                                    ║
║                                                         ║
╚═════════════════════════════════════════════════════════╝
```

### Back

```
╔═════════════════════════════════════════════════════════╗
║              [ LANGUAGE BADGE ]                         ║
║              T A R G E T   I T E M                      ║
║                                                         ║
║              — A N S W E R —                            ║
║                                                         ║
║       Sentence with [answer] highlighted in gold        ║
║       en:  English translation                          ║
║                                                         ║
║       A.  distractor                                    ║
║       B.  correct answer ✔  (gold-highlighted)          ║
║       C.  distractor                                    ║
║       D.  distractor                                    ║
║                                                         ║
║              — W H Y —                                  ║
║                                                         ║
║       One-sentence explanation of the rule that         ║
║       makes the answer correct — references tense,      ║
║       aspect, agreement, collocation, register, or      ║
║       counter classification as appropriate.            ║
║                                                         ║
╚═════════════════════════════════════════════════════════╝
```

The `— W H Y —` section renders conditionally — if a card has no reasoning, that section is hidden cleanly rather than showing empty space. (At the current release every card has reasoning, so the block always appears.)

See [`samples/`](../es/samples/) folders per language for actual rendered cards.

---

## Distractor design per deck type

| Deck | Distractor strategy |
|------|---------------------|
| **Function-Word** | Sentence-aware: Gemini picks 3 other function words that *could plausibly fit syntactically* in this sentence but change the meaning or fail subtly. Forces semantic attention. |
| **Grammar-Pattern** | Typed (3 kinds): **tense** (other tense of same verb), **confusable** (look-alike grammar pattern), **register** (formality-mismatched variant). Each card mixes 2-3 of these types. |
| **Sentence-Mining** | Re-uses the vocab card's pre-built recognition distractors: 1 near-semantic (BGE-M3 cosine), 1 frequency-decile-matched, 1 form-confusable. |
| **Conjugation** | Other legitimate forms of the *same* verb (e.g. for `hablé`: `hablo`, `hablaba`, `hablaré`). Maximum pedagogical signal — the card *forces* tense recognition rather than testing vocab. |
| **Counter / Measure-Word** | Other counters that classify *different* noun types (e.g. for books-counter `冊`: `匹` for animals, `枚` for paper, `個` generic). |
| **Honorifics / Register** | Other register variants of the same proposition — same meaning, wrong speech level for the stated context (e.g. for a formal email: 3 casual alternatives are wrong because of register, not meaning). |
| **Synonyms** | 1 verified synonym + 3 same-POS distractors drawn from different regions of the embedding space (mid + far cosine), ensuring the correct answer is the only true synonym. |

---

## The reasoning ("W H Y") block

Every cloze-MCQ card has one sentence (max ~22 words) explaining *why* the answer fits. The prompt template varies by deck type to focus the explanation on the right axis:

- Function & Grammar: rule that makes the answer correct grammatically.
- Sentence-Mining: meaning + collocation context.
- Conjugation: tense / aspect / mood / agreement.
- Counter: what noun type the counter classifies.
- Register: speech-level + honorific suffix.
- Synonyms: the shared semantic core (this is the *verifier* response, so it doubles as evidence that the synonym pair was approved).

All reasoning is generated by **Gemini 3 Flash Preview** via OpenRouter, with:
- temperature 0.2 (low — we want consistent, rule-citing answers)
- max-tokens 110 (forces brevity)
- prompt-level rules: no markdown, no quoting the answer, no restating the English

If the model errors (rate limit, timeout), the card is requeued in a resume pass until 100% coverage is reached.

---

## Answer position randomization

For every card, the 4 options are shuffled with a **per-card deterministic seed** built from `(spec, lang, rank/key, sentence)` — this means:

- Rebuilds produce identical decks (reproducibility for QA).
- Positions are independent and uniformly distributed across cards (no bias).
- The correct answer lands on A/B/C/D with ~25% probability each — verified by χ² test on every deck before release.

---

## Anki model IDs

Each deck type has a stable model ID so re-imports don't duplicate models:

```
Function-Word Cloze    (Golden Age)       2_345_500_001
Grammar-Pattern Cloze  (Golden Age v2)    2_345_500_003
Sentence-Mining Cloze  (Golden Age v1)    2_345_500_004
Conjugation Drill      (Golden Age v1)    2_345_500_005
Counter / Measure-Word (Golden Age v1)    2_345_500_006
Synonym Selection      (Golden Age v1)    2_345_500_007
Honorifics / Register  (Golden Age v1)    2_345_500_008
```

Deck IDs are also stable per `(deck-type, language)` so you can install all decks of all types side-by-side without GUID collisions.
