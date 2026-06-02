# Architecture

End-to-end data flow for building one language deck.

```
┌─────────────────────────────────────────────────────────────────────┐
│                       SOURCE CORPORA                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   HermitDave/FrequencyWords (OpenSubtitles 2018)                    │
│        es_50k.txt   zh_50k.txt   ko_50k.txt   fr_50k.txt            │
│                                                                     │
│   OPUS OpenSubtitles 2018 raw Japanese corpus (100 MB)              │
│        → re-tokenize with fugashi+UniDic → ja_50k.txt (42.8K lemmas)│
│                                                                     │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    PER-WORD ENRICHMENT                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   gloss:   jamdict (ja) / CC-CEDICT (zh) / Wiktionary / Gemini      │
│   IPA:     espeak+epitran (es) / fugashi+OpenJTalk (ja)             │
│            pypinyin+pinyin-to-ipa (zh) / Hangul rules (ko)          │
│   pinyin:  pypinyin (zh only)                                       │
│   POS:     spaCy es_core_news_sm (es) / fugashi+UniDic (ja)         │
│            jieba (zh) / kiwipiepy (ko)                              │
│   example: Gemini-3-flash-preview (one short sentence + EN trans)   │
│                                                                     │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                  SEMANTIC EMBEDDINGS                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   BAAI/bge-m3 (multilingual, 1024-dim)                              │
│   Input format:  "{word} — {gloss}"                                 │
│   Hardware:      single L4 GPU (~25 s for 50,000 words)             │
│   Output:        word-embeddings-{lang}.npz                         │
│                                                                     │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                  DISTRACTOR SELECTION                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   For each card, pick 5 distractors:                                │
│     • 2 near-semantic  — top BGE-M3 cosine within same POS+decile   │
│     • 2 same-decile fill — random within frequency band             │
│     • 1 form-confusable — Levenshtein / shared-char neighbor        │
│                                                                     │
│   Rotation cap: each pool word used ≤ 8 times as a distractor       │
│                 (within-deck), with "prefer least-used" tiebreaker  │
│                                                                     │
│   Implementation: GPU-batched torch on L4 (~1 min for 10K cards)    │
│                                                                     │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                     PACKAGING                                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   genanki → three .apkg files per language:                         │
│     • Vocab-{Lang}-OpenSubs.apkg              (classic)             │
│     • Vocab-MCQ-Recognition-{Lang}-...apkg    (def → word)          │
│     • Vocab-MCQ-Recall-{Lang}-...apkg         (word → def)          │
│                                                                     │
│   Uploaded to gs://aol-language-decks-v2 with per-object publicRead │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## Why this layering

**Source separation.** Frequency comes from a real corpus (subtitle text), glosses come from lexicographic sources, sentences come from a model. Each layer is independently verifiable and replaceable.

**Embedding-driven distractors > bag-of-words.** Lemma `feliz` (happy) → BGE-M3 surfaces `alegre / contento / felicidad`. Lemma similarity by gloss-token overlap (Jaccard) misses synonyms with disjoint English tokens. The model captures cross-language semantic neighborhoods that simple text matching can't.

**Rotation cap.** Without it, popular words would dominate distractor slots and users learn "X is always a distractor, never the answer." With cap=8 on a 10K pool × 5 distractors per card, we use the full pool with mean ~5 uses per word.

**GPU only for the matrix bits.** Embedding and cosine selection are GPU-natural; everything else (tokenization, gloss lookup, sentence gen) runs on CPU or via API.
