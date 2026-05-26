# SPEC 1 — Sentence-Mining Cloze — Korean sample

_Real card sampled from the production deck._

```
╔════════════ SPEC 1 — Sentence-Mining Cloze · Korean · FRONT ═════════════╗
║                                                                          ║
║ target:  내가  (rank 1, NOUN)                                              ║
║ gloss:   allomorph of 나 (na, “(informal) I”) before -가 (-ga, “nominat    ║
║                                                                          ║
║                             — P R O M P T —                              ║
║                                                                          ║
║ ____ 어제 도서관에서 재미있는 소설 책을 한 권 빌렸어.                                        ║
║                                                                          ║
║ en:  I borrowed an interesting novel from the library yesterday.         ║
║                                                                          ║
║                               — OPTIONS —                                ║
║    A.  나는                                                                ║
║    B.  내가                                                                ║
║    C.  너랑                                                                ║
║    D.  내                                                                 ║
║                                                                          ║
╚══════════════════════════════════════════════════════════════════════════╝
╔═════════════ SPEC 1 — Sentence-Mining Cloze · Korean · BACK ═════════════╗
║                                                                          ║
║                             — A N S W E R —                              ║
║                                                                          ║
║ 내가 어제 도서관에서 재미있는 소설 책을 한 권 빌렸어.                                          ║
║                                                                          ║
║ en:  I borrowed an interesting novel from the library yesterday.         ║
║                                                                          ║
║                               — OPTIONS —                                ║
║    A.  나는                                                                ║
║  ✔ B.  내가                                                                ║
║    C.  너랑                                                                ║
║    D.  내                                                                 ║
║                                                                          ║
║                                — W H Y —                                 ║
║                                                                          ║
║ The informal first-person pronoun na changes to nae when combined with   ║
║ the subject marker ga to identify the sentence's active agent.           ║
║                                                                          ║
╚══════════════════════════════════════════════════════════════════════════╝
```
