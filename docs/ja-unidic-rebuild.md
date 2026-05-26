# Japanese — UniDic Re-tokenize

How we fixed the "stem soup" problem in the original Japanese vocabulary list.

## The problem

The HermitDave OpenSubtitles JA frequency list was produced with **MeCab + IPAdic**. IPAdic splits inflected forms into separate stem and ending tokens. So in their list:

- `走る` (run, dictionary form) doesn't appear — instead you see `走` (root) and `る` (ending) as separate entries
- `食べる` (eat) → `食べ` + `る`
- `美しい` (beautiful) → may appear, or split as `美し` + `い`
- `猫` (cat) is missing entirely from the top 50K (low frequency in the IPAdic-tokenized stream)

For a learner, this is broken. Their textbook says "kuru means come," but the frequency list has `来` and `い` as separate ranked items.

## The fix

Re-tokenize the raw OpenSubtitles 2018 Japanese corpus ourselves with **fugashi + UniDic**. UniDic stores the dictionary form (lemma) in `feature.lemma`, so we can group all inflections back to a single canonical entry.

```
input:  彼は走った美しい猫を見た。
fugashi+IPAdic tokens:  彼 は 走っ た 美しい 猫 を 見 た 。
fugashi+UniDic lemmas:  彼 は 走る た 美しい 猫 を 見る た 。
                              ▲              ▲
                       dict form         dict form
```

We count **lemmas** (not surface tokens), skip particles + punctuation + numerals + auxiliary stems, and sort by descending count.

## Results

**Pool size:** 34,504 stem-soup entries → **42,833 clean dictionary forms**

**Top-30 most-frequent lemmas** in the new list:

```
為る (suru, do)            居る (iru, be)              無い (nai, no)
私 (watashi, I)            何 (nani, what)            事 (koto, thing)
有る (aru, be/inanimate)   良い (yoi, good)           貴方 (anata, you)
言う (iu, say)             其れ (sore, that)          彼 (kare, he)
行く (iku, go)             そう (sou, so)             来る (kuru, come)
俺 (ore, I/masc)           どう (dou, how)            成る (naru, become)
分かる (wakaru, understand) 君 (kimi, you)             見る (miru, see)
遣る (yaru, do/give)       呉れる (kureru, give)       此れ (kore, this)
出来る (dekiru, can do)    此処 (koko, here)           御前 (omae, you)
思う (omou, think)         知る (shiru, know)          誰 (dare, who)
```

These are all **dictionary forms** any learner's textbook will recognize.

## Coverage check on common learner vocab

| Word | Meaning | Rank in new list | Rank in old (IPAdic) |
|------|---------|-----------------:|-----------------------:|
| 話す | to talk; to speak | **48** | not present |
| 家 | house | **88** | not present |
| 食べる | to eat | **198** | not present |
| 寝る | to sleep | **281** | not present |
| 学校 | school | **386** | not present |
| 走る | to run | **443** | not present |
| 美しい | beautiful | **487** | not present |
| 友達 | friend | **210** | not present |
| 書く | to write | **263** | not present |
| 読む | to read | **314** | not present |
| 飲む | to drink | **189** | not present |
| 泳ぐ | to swim | **1,776** | not present |
| 遊ぶ | to play | **752** | not present |
| 働く | to work | **290** | not present |
| 猫 | cat | **965** | not present |

Every common verb and noun a JLPT N5 learner encounters in lesson 1 is now present and properly ranked.

## How long it takes

3.17 M lines of raw Japanese subtitle text → 21 M tokens → 8.76 M kept (41.5% after POS filter) → 60,456 unique lemmas → 42,833 entries after `min_count=2` cutoff.

Wall clock: **127 seconds** on a single CPU.

## Why we kept the old list

For transparency and reproducibility, the original IPAdic-tokenized list is kept at `ja/words/ja-frequency-list-ipadic-original.txt`. Anyone can diff the two to see exactly which lemmas were missing/different.

## What this changes downstream

- **Card targets:** the 10,000 cards in the Japanese deck come from the top 10,000 of this new list, so dictionary-form verbs and adjectives are now flashcard-able.
- **Distractor pool:** 42,833 candidate distractors instead of 34,504 (and with cleaner semantics — no stems competing with their endings).
- **BGE-M3 embeddings:** re-computed for the new lemma set so semantic neighbors are clean (e.g., `救い出す` finds `運び出す, 掘り出す` — all `〜出す` compound verbs).
