# Japanese — Vocabulary Deck

10,000 cards covering the top 10,000 most frequent Japanese **dictionary-form lemmas** in the OpenSubtitles 2018 corpus.

> **v2 change:** the Japanese frequency list was rebuilt with **fugashi + UniDic** to surface dictionary forms (`走る`, `食べる`, `美しい`) instead of inflectional stems (`走` + `る` etc.). See [`../docs/ja-unidic-rebuild.md`](../docs/ja-unidic-rebuild.md) for the full story.

## Downloads

| Deck | Direction | Cards | File |
|------|-----------|------:|------|
| **Classic vocab** | word → meaning (open recall) | 10,000 | [⬇ Vocab-Japanese-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ja/Vocab-Japanese-OpenSubs.apkg) (5.2 MB) |
| **MCQ recognition** | definition → word (pick 1 of 6) | 10,000 | [⬇ Vocab-MCQ-Recognition-Japanese-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ja/Vocab-MCQ-Recognition-Japanese-OpenSubs.apkg) (40 MB) |
| **MCQ recall** | word → definition (pick 1 of 6) | 10,000 | [⬇ Vocab-MCQ-Recall-Japanese-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ja/Vocab-MCQ-Recall-Japanese-OpenSubs.apkg) (23 MB) |

## Source

- **Frequency list:** OpenSubtitles 2018 raw Japanese corpus from [OPUS](http://opus.nlpl.eu/OpenSubtitles-v2018.php) (3.17 M lines, 100 MB), **re-tokenized by us** with fugashi + UniDic. Result: 42,833 dictionary-form lemmas at [`words/ja-frequency-list-unidic.txt`](words/ja-frequency-list-unidic.txt).
- **Original IPAdic list** (the upstream HermitDave version, 34,504 stem-soup entries) is kept for transparency at [`words/ja-frequency-list-ipadic-original.txt`](words/ja-frequency-list-ipadic-original.txt).
- **Glosses:** [jamdict](https://github.com/neocl/jamdict) (wrapping [JMdict](http://www.edrdg.org/jmdict/edict.html)) (priority) → Wiktionary → Gemini fallback
- **IPA:** [fugashi](https://github.com/polm/fugashi) + [OpenJTalk](http://open-jtalk.sourceforge.net/) for phonetic transcription
- **POS:** fugashi + UniDic (`feature.pos1` → universal coarse tag)
- **Example sentences:** Google Gemini 3 Flash Preview (one short canonical sentence per word + English translation)
- **Semantic distractors:** BAAI/bge-m3 (1024-dim multilingual embedding)

## Coverage

- 10,000 cards with gloss (100%)
- 10,000 cards with example sentence (100% Gemini-curated)
- 10,000 cards with IPA
- 6-option MCQ for both directions, 100% coverage
- Common learner vocab now properly ranked:
  - 話す (talk) #48 · 家 (house) #88 · 食べる (eat) #198 · 寝る (sleep) #281
  - 学校 (school) #386 · 走る (run) #443 · 美しい (beautiful) #487 · 猫 (cat) #965

## Sample cards

- [Classic vocab card (back side)](samples/classic-vocab-card.md)
- [MCQ Recognition card](samples/mcq-recognition-card.md)
- [MCQ Recall card](samples/mcq-recall-card.md)

## How to import

1. Install [Anki Desktop](https://apps.ankiweb.net/) (free).
2. Download the `.apkg` file.
3. In Anki: **File → Import** → select the file.
