# Spanish — Vocabulary Deck

10,000 cards covering the top 10,000 most frequent Spanish words in the OpenSubtitles 2018 corpus.

## Downloads

| Deck | Direction | Cards | File |
|------|-----------|------:|------|
| **Classic vocab** | word → meaning (open recall) | 10,000 | [⬇ Vocab-Spanish-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/es/Vocab-Spanish-OpenSubs.apkg) (4.2 MB) |
| **MCQ recognition** | definition → word (pick 1 of 6) | 10,000 | [⬇ Vocab-MCQ-Recognition-Spanish-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/es/Vocab-MCQ-Recognition-Spanish-OpenSubs.apkg) (36 MB) |
| **MCQ recall** | word → definition (pick 1 of 6) | 10,000 | [⬇ Vocab-MCQ-Recall-Spanish-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/es/Vocab-MCQ-Recall-Spanish-OpenSubs.apkg) (21 MB) |

## Source

- **Frequency list:** [HermitDave/FrequencyWords — es_50k](https://github.com/hermitdave/FrequencyWords/blob/master/content/2018/es/es_50k.txt) (OpenSubtitles 2018). Included verbatim at [`words/es-frequency-list.txt`](words/es-frequency-list.txt).
- **Glosses:** Wiktionary (priority) → Gemini fallback for the long tail
- **IPA:** [espeak-ng](https://github.com/espeak-ng/espeak-ng) + [epitran](https://github.com/dmort27/epitran)
- **POS:** spaCy [`es_core_news_sm`](https://spacy.io/models/es) (universal-dependencies tagset)
- **Example sentences:** Google Gemini 3 Flash Preview (one short canonical sentence per word + English translation)
- **Semantic distractors:** BAAI/bge-m3 (1024-dim multilingual embedding)

## Coverage

- 10,000 cards with gloss
- 10,000 cards with example sentence (100% Gemini-curated)
- 10,000 cards with IPA
- 6-option MCQ for both directions, 100% coverage

## Sample cards

- [Classic vocab card (back side)](samples/classic-vocab-card.md)
- [MCQ Recognition card](samples/mcq-recognition-card.md)
- [MCQ Recall card](samples/mcq-recall-card.md)

## How to import

1. Install [Anki Desktop](https://apps.ankiweb.net/) (free).
2. Download the `.apkg` file you want.
3. In Anki: **File → Import** → select the file.

Each deck creates a sub-deck under `Vocabulary::Spanish` (or `Vocabulary-MCQ-Recognition::Spanish`, etc.), so you can install all three side-by-side without conflicts.
