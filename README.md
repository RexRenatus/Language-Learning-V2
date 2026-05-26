# Language-Learning-V2

Curated multilingual vocabulary flashcards for **Spanish, Japanese, Chinese (Mandarin), and Korean**.

10,000 cards per language, three card formats per word: **classic vocab**, **MCQ recognition** (definition → word), and **MCQ recall** (word → definition). Distractors selected by semantic similarity (BGE-M3 embeddings) with rotation fairness. Example sentences curated by Gemini, not pulled from a single source corpus.

> Previous version: [DEPRECATED-Language-Learning-Journey](https://github.com/RexRenatus/DEPRECATED-Language-Learning-Journey) (archived).

---

## What's new in v2

| Aspect | v1 | v2 |
|--------|----|----|
| Card formats | Classic only | Classic + MCQ recognition + MCQ recall |
| Distractors | n/a | 6-option MCQ; 2 near-semantic + 2 frequency-decile + 1 form-confusable |
| Distractor similarity | n/a | **BGE-M3** 1024-dim multilingual cosine |
| Distractor rotation | n/a | Capped at 8 uses per pool word; full pool covered |
| Example sentences | Harry Potter corpus only | **Gemini-curated** short canonical sentences (8–14 words) with English translations |
| Japanese tokenization | IPAdic stems (`走` + `る`) | **UniDic dictionary forms** (`走る` as a single lemma) |
| Japanese vocab pool | 34,504 stem-soup entries | 42,833 clean lemmas |
| Decks per language | 1 | 3 |
| Total study items | 40,000 | **120,000** across 12 decks |

For the Japanese rebuild story, see [`docs/ja-unidic-rebuild.md`](docs/ja-unidic-rebuild.md).

---

## Downloads

All decks are hosted on Google Cloud Storage. Direct download — no signup. Import into Anki Desktop with **File → Import**.

| Language | Classic Vocab | MCQ Recognition | MCQ Recall |
|----------|---------------|-----------------|------------|
| **Spanish**  | [⬇ Vocab-Spanish.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/es/Vocab-Spanish-OpenSubs.apkg) (4.2 MB) | [⬇ MCQ Recognition.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/es/Vocab-MCQ-Recognition-Spanish-OpenSubs.apkg) (36 MB) | [⬇ MCQ Recall.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/es/Vocab-MCQ-Recall-Spanish-OpenSubs.apkg) (21 MB) |
| **Japanese** | [⬇ Vocab-Japanese.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ja/Vocab-Japanese-OpenSubs.apkg) (5.2 MB) | [⬇ MCQ Recognition.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ja/Vocab-MCQ-Recognition-Japanese-OpenSubs.apkg) (40 MB) | [⬇ MCQ Recall.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ja/Vocab-MCQ-Recall-Japanese-OpenSubs.apkg) (23 MB) |
| **Chinese**  | [⬇ Vocab-Chinese.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/zh/Vocab-Chinese-OpenSubs.apkg) (5.5 MB) | [⬇ MCQ Recognition.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/zh/Vocab-MCQ-Recognition-Chinese-OpenSubs.apkg) (40 MB) | [⬇ MCQ Recall.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/zh/Vocab-MCQ-Recall-Chinese-OpenSubs.apkg) (27 MB) |
| **Korean**   | [⬇ Vocab-Korean.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ko/Vocab-Korean-OpenSubs.apkg) (4.7 MB) | [⬇ MCQ Recognition.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ko/Vocab-MCQ-Recognition-Korean-OpenSubs.apkg) (40 MB) | [⬇ MCQ Recall.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/ko/Vocab-MCQ-Recall-Korean-OpenSubs.apkg) (25 MB) |

Per-language pages with sample cards: **[Spanish](es/) · [Japanese](ja/) · [Chinese](zh/) · [Korean](ko/)**

---

## What's in each deck

- **Classic vocab card** — Word + IPA (and Pinyin for Chinese) on the front; English meaning, example sentence, top collocations on the back.
- **MCQ Recognition** — English definition shown; user picks the matching target-language word out of six options.
- **MCQ Recall** — Target-language word + IPA shown; user picks the matching English definition out of six options.

Each card is built from a single high-frequency word. Coverage:

| Language | Cards | 100% with gloss | 100% with example | 100% with MCQ options |
|----------|-------|-----------------|-------------------|----------------------|
| Spanish | 10,000 | ✓ | ✓ | ✓ |
| Japanese | 10,000 | ✓ | ✓ | ✓ |
| Chinese | 10,000 | ✓ | ✓ | ✓ |
| Korean | 10,000 | 99.8% | 99.8% | ✓ |

---

## Frequency lists (the 50K word source)

Each language folder ships the underlying frequency list used to select the top 10K cards:

- [`es/words/es-frequency-list.txt`](es/words/es-frequency-list.txt) — 50,000 entries
- [`ja/words/ja-frequency-list-unidic.txt`](ja/words/ja-frequency-list-unidic.txt) — **42,833 UniDic dictionary forms** (our re-tokenize)
- [`ja/words/ja-frequency-list-ipadic-original.txt`](ja/words/ja-frequency-list-ipadic-original.txt) — 34,504 entries, original IPAdic source (kept for transparency)
- [`zh/words/zh-frequency-list.txt`](zh/words/zh-frequency-list.txt) — 50,000 entries
- [`ko/words/ko-frequency-list.txt`](ko/words/ko-frequency-list.txt) — 50,000 entries

Format: `word count\n` per line, sorted descending by corpus frequency.

---

## How it was built

High-level data flow:

```
OpenSubtitles 2018 corpus (HermitDave FrequencyWords + raw via OPUS for JA)
        │
        ▼
 Top-N frequency selection ───►  Free dictionaries (CC-CEDICT, jamdict, Wiktionary)
        │                                        │
        ▼                                        ▼
 Per-word enrichment ◄────────────  Gemini fallback for the long tail
        │
        ├─► IPA per word (espeak/epitran/fugashi+OpenJTalk/pypinyin/Korean rules)
        ├─► Pinyin (Chinese only)
        ├─► Example sentence + English translation (Gemini)
        └─► POS tag (spaCy / fugashi+UniDic / jieba / kiwipiepy)
        │
        ▼
 BGE-M3 embedding per word (one-shot L4 GPU job, 1024-dim multilingual)
        │
        ▼
 GPU distractor selector (batched cosine; rotation-capped)
        │
        ▼
 Anki .apkg packaging (genanki) → distributed via GCS
```

See:
- [`docs/architecture.md`](docs/architecture.md) — full pipeline
- [`docs/distractor-design.md`](docs/distractor-design.md) — the 2/2/1 recipe + rotation math
- [`docs/ja-unidic-rebuild.md`](docs/ja-unidic-rebuild.md) — why and how we rebuilt the Japanese vocab pool

---

## Attribution & sources

This project stands on the shoulders of:

- **OpenSubtitles 2018** (via [OPUS](http://opus.nlpl.eu/OpenSubtitles-v2018.php) and [HermitDave/FrequencyWords](https://github.com/hermitdave/FrequencyWords)) — raw subtitle corpora and frequency lists. Licensed for academic and research use.
- **[BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3)** — multilingual embedding model (MIT license).
- **[Google Gemini](https://ai.google.dev/) (via OpenRouter)** — example-sentence generation and definition fallback.
- **Tokenizers:**
  - **[fugashi](https://github.com/polm/fugashi)** + **[unidic-lite](https://github.com/polm/unidic-lite)** for Japanese (GPL/MIT)
  - **[jieba](https://github.com/fxsjy/jieba)** for Chinese (MIT)
  - **[spaCy](https://spacy.io/) `es_core_news_sm`** for Spanish (MIT)
  - **[kiwipiepy](https://github.com/bab2min/kiwipiepy)** for Korean (LGPL)
- **Dictionaries** for free-tier gloss lookup:
  - **[CC-CEDICT](https://www.mdbg.net/chinese/dictionary?page=cc-cedict)** (CC-BY-SA 4.0) for Chinese
  - **[jamdict](https://github.com/neocl/jamdict)** wrapping JMdict (Creative Commons BY-SA)
  - **[Wiktionary](https://www.wiktionary.org/)** REST + wikitext (CC-BY-SA)
- **IPA generation:**
  - **[espeak-ng](https://github.com/espeak-ng/espeak-ng)** + **[epitran](https://github.com/dmort27/epitran)** for Spanish
  - **fugashi + [OpenJTalk](http://open-jtalk.sourceforge.net/)** for Japanese
  - **[pypinyin](https://github.com/mozillazg/python-pinyin) + [pinyin-to-ipa](https://github.com/stefanstaplerstudio/pinyin-to-ipa)** for Chinese
  - Rule-based Hangul-to-IPA for Korean
- **[genanki](https://github.com/kerrickstaley/genanki)** — `.apkg` packaging (MIT).

All third-party libraries used in their original form, no modifications.

---

## License

MIT — see [LICENSE](LICENSE).

The included frequency lists (`*/words/*.txt`) are derived from OpenSubtitles 2018 via HermitDave/FrequencyWords and are redistributed under the spirit of that project's terms. If you build on these decks, please credit OpenSubtitles 2018 and HermitDave/FrequencyWords.
