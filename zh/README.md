# Chinese (Mandarin) — Vocabulary Deck

10,000 cards covering the top 10,000 most frequent Chinese words in the OpenSubtitles 2018 corpus.

## Downloads

| Deck | Direction | Cards | File |
|------|-----------|------:|------|
| **Classic vocab** | word → meaning (open recall) | 10,000 | [⬇ Vocab-Chinese-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/zh/Vocab-Chinese-OpenSubs.apkg) (5.5 MB) |
| **MCQ recognition** | definition → word (pick 1 of 6) | 10,000 | [⬇ Vocab-MCQ-Recognition-Chinese-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/zh/Vocab-MCQ-Recognition-Chinese-OpenSubs.apkg) (40 MB) |
| **MCQ recall** | word → definition (pick 1 of 6) | 10,000 | [⬇ Vocab-MCQ-Recall-Chinese-OpenSubs.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/zh/Vocab-MCQ-Recall-Chinese-OpenSubs.apkg) (27 MB) |

## Source

- **Frequency list:** [HermitDave/FrequencyWords — zh_50k](https://github.com/hermitdave/FrequencyWords/blob/master/content/2018/zh/zh_50k.txt) (OpenSubtitles 2018). Included verbatim at [`words/zh-frequency-list.txt`](words/zh-frequency-list.txt).
- **Glosses:** [CC-CEDICT](https://www.mdbg.net/chinese/dictionary?page=cc-cedict) (priority) → Wiktionary → Gemini fallback
- **IPA:** [pypinyin](https://github.com/mozillazg/python-pinyin) → [pinyin-to-ipa](https://github.com/stefanstaplerstudio/pinyin-to-ipa)
- **Pinyin:** pypinyin with tone marks (`nǐ hǎo` form)
- **POS:** [jieba](https://github.com/fxsjy/jieba) `posseg` (ICTCLAS-derived tags → universal coarse tag)
- **Example sentences:** Google Gemini 3 Flash Preview (one short canonical sentence per word + English translation)
- **Semantic distractors:** BAAI/bge-m3 (1024-dim multilingual embedding)

## Coverage

- 10,000 cards with gloss (100%)
- 10,000 cards with example sentence (100% Gemini-curated)
- 10,000 cards with IPA + tone-marked pinyin
- 6-option MCQ for both directions, 100% coverage
- Cards show both **IPA** and **pinyin** on the front for full pronunciation cue

## Sample cards

- [Classic vocab card (back side)](samples/classic-vocab-card.md)
- [MCQ Recognition card](samples/mcq-recognition-card.md)
- [MCQ Recall card](samples/mcq-recall-card.md)

## How to import

1. Install [Anki Desktop](https://apps.ankiweb.net/) (free).
2. Download the `.apkg` file.
3. In Anki: **File → Import** → select the file.
