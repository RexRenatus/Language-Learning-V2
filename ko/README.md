# Korean — Vocabulary Deck

10,000 cards covering the top 10,000 most frequent Korean words in the OpenSubtitles 2018 corpus.

## Downloads

| Deck | Direction | Cards | File |
|------|-----------|------:|------|
| **Classic vocab** | word → meaning (open recall) | 10,000 | [⬇ Vocab-Korean-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ko/Vocab-Korean-OpenSubs.apkg) (4.7 MB) |
| **MCQ recognition** | definition → word (pick 1 of 6) | 10,000 | [⬇ Vocab-MCQ-Recognition-Korean-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ko/Vocab-MCQ-Recognition-Korean-OpenSubs.apkg) (40 MB) |
| **MCQ recall** | word → definition (pick 1 of 6) | 10,000 | [⬇ Vocab-MCQ-Recall-Korean-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ko/Vocab-MCQ-Recall-Korean-OpenSubs.apkg) (25 MB) |

## Source

- **Frequency list:** [HermitDave/FrequencyWords — ko_50k](https://github.com/hermitdave/FrequencyWords/blob/master/content/2018/ko/ko_50k.txt) (OpenSubtitles 2018). Included verbatim at [`words/ko-frequency-list.txt`](words/ko-frequency-list.txt).
- **Glosses:** Wiktionary (priority) → Gemini fallback for the long tail
- **IPA:** Rule-based Hangul-to-IPA mapping (assimilation + final-consonant phonotactics)
- **POS:** [kiwipiepy](https://github.com/bab2min/kiwipiepy) (Kiwi morphological analyzer, Sejong tagset → universal coarse tag)
- **Example sentences:** Google Gemini 3 Flash Preview (one short canonical sentence per word + English translation)
- **Semantic distractors:** BAAI/bge-m3 (1024-dim multilingual embedding)

## Coverage

- 10,000 cards (99.8% with gloss)
- 9,979 cards with example sentence (99.8% Gemini-curated)
- 10,000 cards with IPA
- 6-option MCQ for both directions, 100% coverage

## Sample cards

- [Classic vocab card (back side)](samples/classic-vocab-card.md)
- [MCQ Recognition card](samples/mcq-recognition-card.md)
- [MCQ Recall card](samples/mcq-recall-card.md)

## How to import

1. Install [Anki Desktop](https://apps.ankiweb.net/) (free).
2. Download the `.apkg` file.
3. In Anki: **File → Import** → select the file.
