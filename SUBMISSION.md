# HW1 submission

**Name:Danil**
**Student ID:S23069071**
**Group:CSS4007-ENG-10**
**Repository:CardiganFlanagan**

## AI tool disclosure

State which AI tools you used and for what. Expected and fine; undisclosed use
is not.

>
Used Claude (Anthropic) to debug environment/API errors (tiktoken download failure, OpenAI/OpenRouter rate limits and credit errors, max_tokens vs max_completion_tokens parameter mismatch across providers) and to get code review / explanations

---

## Sublab Easy — the registration bot and its bill

**How I laid the catalogue out inside the system prompt, and why:**

>Разложил на студента, правила и курсы для точечной работы с текстом, чтобы мы могли более точную информацию дать иишке

**My turn 5 (Kazakh or Russian):**

>Я студент третьего курса. К каким курсам у меня все еще есть доступ для регистрации?

### Run 1 — OpenAI, `gpt-5.6-luna`

| Turn | Input tokens | Output tokens | Cost $ |
|---|---|---|---|
| 1 |491 |492 |0.000689 |
| 2 |730 |198 |0.000384 |
| 3 |851 |99 |0.000289 |
| 4 |934 |29 |0.000222 |
| 5 |989 |301 |0.000559 |
| **total** |3995 |1119 |0.002142 |

### Run 2 — OpenRouter, `google/gemma-4-26b-a4b-it:free`

| Turn | Input tokens | Output tokens | Cost $ |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| **total** | | | |

Two separate attempts (on different days/times) both failed on turn 1 with 429 temporarily rate-limited upstream from Google AI Studio, before any usage data was returned.

### Turn 4, verbatim

The turn where you asked for CSS-4090, which does not exist. Paste both replies
exactly as they came back — do not tidy them.

**OpenAI:**

```
I can't add CSS-4090 — Quantum Machine Learning because it is not in the course catalogue provided.
```

**OpenRouter:**

```
N/A — request failed with 429 (rate-limited upstream) before turn 4 was reached.
```

### Written answers

**1. The two providers used almost identical code. What actually changed, and
what did not?**

>different only base_url (и ключ), class OpenAI, method .chat.completions.create(), structure messages — every thing other is same

**2. Why did the input token count climb on every turn when your questions
stayed roughly the same length? Use the numbers from your own table. What
happens to the bill at fifty turns?**

>because ai remember all our chat and every turn take full chat before answering. in 50 turn it will be like 1600 – 1700.

**3. Turn 4: did the bot refuse, or did it invent CSS-4090?** If it refused, what
in your system prompt held the line? If it invented, what did it make up —
credits, a room, an instructor?

>it is refused because in our promt there says 
        " Never invent a course, course code, credits, "
        "schedule, instructor, or other catalogue information.",
        "If a requested course is not in the catalogue, refuse the request "

**4. Where else was either bot wrong?** Turn 2 asks for two courses that meet at
the same hour; two courses in the catalogue are full. Did the bots notice?

>bot saw time collision from turn 1 and implement it in turn 2 and turn 3

---

## Sublab Medium — one task, six models

Paste the per-model summary printed by `correct_kazakh.py`:

| Model | Exact | Failed | Tokens | Cost $ |
|---|---|---|---|---|
| google/gemma-4-26b-a4b-it:free |0 |6 |540 |0.00000 |
| qwen/qwen3.8-27b |6 |2 |16245 |0.04828 |
| deepseek/deepseek-v4-flash-0731 |7 |0 |13981 |0.00365 |
| gpt-5.6-luna |4 |0 |4538 |0.00431 |
| gpt-5.6-terra |3 |0 |2823 |0.02251 |
| gpt-5.6-sol |4 |0 |2713 |0.05296 |

### Which error types did each model repair?

Rows are error labels, columns are models. Write "yes", "no" or "partial".

| Error type | gemma | qwen | deepseek | luna | terra | sol |
|---|---|---|---|---|---|---|
| kaz_to_rus | | | | | | |
| latin_homoglyph | | | | | | |
| drop_hyphen | | | | | | |
| join_words | | | | | | |
| double_letter | | | | | | |

**The `latin_homoglyph` row: what happened?** Describe what you observed. The
explanation is Sublab Harder's job, not this one's.

>latin homoglyph show stsble resurt from every models

**Where a model returned good Kazakh that was not identical to the original,
say so here.** Exact match is not correctness.

>gpt 5.6 sol show us most results that not identical from original

**Cheapest model that was good enough, and why:**

>deepseek/deepseek-v4-flash-0731 show us best perfomance for price (7/8) best from all models

---

## Sublab Harder — open the tokenizer

### A. What a language costs

**`cl100k_base`:**

| Language | Tokens | Chars | Tok/char | × English | $ per 1,000 sentences |
|---|---|---|---|---|---|
| kk |200 |263 |0.760 |3.75 |3.80 |
| ru |129 |277 |0.466 |2.30 |2.33 |
| en |59 |291 |0.203 | 1.00 |1.02 |

**`o200k_base`:**

| Language | Tokens | Chars | Tok/char | × English | $ per 1,000 sentences |
|---|---|---|---|---|---|
| kk |84 |265 |0.319 |1.58 |1.60 |
| ru |74 |277 |0.267 |1.32 |1.34 |
| en |59 |291 |0.203 | 1.00 |1.02 |

### B. What a homoglyph does

One row per `latin_homoglyph` sentence in the dataset. Paste the actual decoded
token strings around the divergence point, not a description of them.

| Sentence id | Foreign char (index, name) | Tokens correct | Tokens corrupted | Δ | Diverges at |
|---|---|---|---|---|---|
|KZ-03 |(0,'A',LATIN CAPITAL LETTER A), (2,'a',LATIN SMALL LETTER A), (5,'t',LATIN SMALL LETTER T) |16 |20 |+4 |0 |
|KZ-08 |	(1,'o',LATIN SMALL LETTER O), (3,'a',LATIN SMALL LETTER A), (9,'T',LATIN CAPITAL LETTER T) |21 |24 |+3 |1 |

**Token pieces around the divergence:**

```
KZ-03
correct  : ['А', 'лая', 'қ', 'тарға', ' ақша']
corrupted: ['A', 'л', 'a', 'я', 'қ']

KZ-08
correct  : ['Д', 'он', 'аль', 'д', ' Т', 'рамп']
corrupted: ['Д', 'o', 'н', 'a', 'л', 'ль']
```

### C. Did it get better?

| Language | cl100k_base | o200k_base | Change |
|---|---|---|---|
| kk |0.760 |0.319 |−0.441 (−58%) |
| ru |0.466 |0.267 |−0.199 (−43%) |
| en |0.203 |0.203 |0 (0%) |

### Written answers

**1. What is the Kazakh tax?** The ratio against English in both encodings, the
dollar figure from A, and how much it changed between the two tokenizers.

>3.75x (cl100k) → 1.58x (o200k), $3.80 → $1.60 for 1000 sentences


**2. Why did the models repair `kaz_to_rus` but struggle with
`latin_homoglyph`?** Both are single-letter substitutions and both look almost
identical on screen. Use your token streams from B as the evidence. Say what the
model actually received in each case.

>KZ-08, after diverging at index 1, tokenizes the rest of the word letter by letter ('o', 'н', 'a', 'л', 'ль'), whereas the correct word was split into bigger meaningful chunks ('он', 'аль', 'д'). The model literally receives a different sequence of tokens

**3. Name one thing this measurement does not explain about your Sublab Medium
results.** You measured OpenAI's tokenizers; three of your six models were not
OpenAI's. What follows, and what would you have to do to close the gap?

>Gemma, Qwen, and DeepSeek use their own tokenizers, which can split homoglyph text differently
