# SPEC 1 — Sentence-Mining Cloze — Chinese sample

_Real card sampled from the production deck._

```
╔════════════ SPEC 1 — Sentence-Mining Cloze · Chinese · FRONT ════════════╗
║                                                                          ║
║ target:  我们  (rank 8, PRON)                                              ║
║ gloss:   we; us; ourselves; our                                          ║
║                                                                          ║
║                             — P R O M P T —                              ║
║                                                                          ║
║ ____明天打算去市中心的公园野餐。                                                       ║
║                                                                          ║
║ en:  We plan to go for a picnic in the city center park tomorrow.        ║
║                                                                          ║
║                               — OPTIONS —                                ║
║    A.  翅膀                                                                ║
║    B.  咱们                                                                ║
║    C.  我们                                                                ║
║    D.  我們                                                                ║
║                                                                          ║
╚══════════════════════════════════════════════════════════════════════════╝
╔════════════ SPEC 1 — Sentence-Mining Cloze · Chinese · BACK ═════════════╗
║                                                                          ║
║                             — A N S W E R —                              ║
║                                                                          ║
║ 我们明天打算去市中心的公园野餐。                                                         ║
║                                                                          ║
║ en:  We plan to go for a picnic in the city center park tomorrow.        ║
║                                                                          ║
║                               — OPTIONS —                                ║
║    A.  翅膀                                                                ║
║    B.  咱们                                                                ║
║  ✔ C.  我们                                                                ║
║    D.  我們                                                                ║
║                                                                          ║
║                                — W H Y —                                 ║
║                                                                          ║
║ As a plural personal pronoun, it provides the necessary subject to       ║
║ perform the collective action of planning and attending a picnic.        ║
║                                                                          ║
╚══════════════════════════════════════════════════════════════════════════╝
```
