# SPEC 1 — Sentence-Mining Cloze — Japanese sample

_Real card sampled from the production deck._

```
╔═══════════ SPEC 1 — Sentence-Mining Cloze · Japanese · FRONT ════════════╗
║                                                                          ║
║ target:  為る  (rank 1, VERB)                                              ║
║ gloss:   to do; to carry out; to perform                                 ║
║                                                                          ║
║                             — P R O M P T —                              ║
║                                                                          ║
║ 私は毎日一時間ぐらい日本語の勉強を____。                                                   ║
║                                                                          ║
║ en:  I study Japanese for about one hour every day.                      ║
║                                                                          ║
║                               — OPTIONS —                                ║
║    A.  処方                                                                ║
║    B.  因る                                                                ║
║    C.  為る                                                                ║
║    D.  為さる                                                               ║
║                                                                          ║
╚══════════════════════════════════════════════════════════════════════════╝
╔════════════ SPEC 1 — Sentence-Mining Cloze · Japanese · BACK ════════════╗
║                                                                          ║
║                             — A N S W E R —                              ║
║                                                                          ║
║ 私は毎日一時間ぐらい日本語の勉強を為る。                                                     ║
║                                                                          ║
║ en:  I study Japanese for about one hour every day.                      ║
║                                                                          ║
║                               — OPTIONS —                                ║
║    A.  処方                                                                ║
║    B.  因る                                                                ║
║  ✔ C.  為る                                                                ║
║    D.  為さる                                                               ║
║                                                                          ║
║                                — W H Y —                                 ║
║                                                                          ║
║ 為る is the standard verb for performing an action, completing the noun-   ║
║ based phrase for studying through a common grammatical collocation.      ║
║                                                                          ║
╚══════════════════════════════════════════════════════════════════════════╝
```
