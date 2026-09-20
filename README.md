# How I Actually Use Local and Cloud AI

**Deutsch:** [LOCAL_AI_WORKFLOW_README_DE.md](LOCAL_AI_WORKFLOW_README_DE.md)


> **Personal practice report – not a general AI benchmark**  
> As of: September 2026

This README describes my own use of AI for data analysis, Python, SQL, documentation, language, and everyday tasks. It is not a scientific benchmark and not a recommendation for any particular hardware. The results apply to **my machine, my models, my settings, and my way of working**.

I'm not an AI engineer and I don't want to pretend to be one. I use AI as a tool. Some things it does extremely fast. Some things it does convincingly wrong. Both are important to know.

I never bought this laptop with local AI in mind — it's simply the laptop I happen to use for that now — and at the time I had no intention of running local AI or becoming an AI engineer. In fact, it does not even have a dedicated GPU, which is one of the most important components for running AI models efficiently.

My most important principle is therefore:

> **AI can speed up work. It does not take responsibility for whether a result is correct.**

That applies to local models just as much as to cloud AI.

## Table of Contents

- [Summary](#summary)
- [1. What this README is – and isn't](#1-what-this-readme-is--and-isnt)
- [2. My system](#2-my-system)
- [3. Getting local execution to work properly first](#3-getting-local-execution-to-work-properly-first)
- [4. Why I currently prefer working directly with Ollama](#4-why-i-currently-prefer-working-directly-with-ollama)
- [5. Human, deterministic code, local AI, and cloud AI](#5-human-deterministic-code-local-ai-and-cloud-ai)
- [6. Why I want local AI at all](#6-why-i-want-local-ai-at-all)
- [7. Why cloud AI still plays a very large role](#7-why-cloud-ai-still-plays-a-very-large-role)
- [8. My actual project workflow](#8-my-actual-project-workflow)
- [9. My local data-analysis workflow](#9-my-local-data-analysis-workflow)
- [10. How I tested the local models](#10-how-i-tested-the-local-models)
- [11. A very important test: `COUNT(*)` was correct](#11-a-very-important-test-count-was-correct)
- [12. Second important test: many-to-many merge](#12-second-important-test-many-to-many-merge)
- [13. Speed: my observed timings](#13-speed-my-observed-timings)
- [14. The models I tested](#14-the-models-i-tested)
- [15. My current model allocation](#15-my-current-model-allocation)
- [16. What I learned from the comparison](#16-what-i-learned-from-the-comparison)
- [17. AI and accuracy](#17-ai-and-accuracy)
- [18. Charts: an area where the human stays essential](#18-charts-an-area-where-the-human-stays-essential)
- [19. How I treat AI-generated code](#19-how-i-treat-ai-generated-code)
- [20. Why I still enjoy coding with AI](#20-why-i-still-enjoy-coding-with-ai)
- [21. My practical quality check](#21-my-practical-quality-check)
- [22. A small example of a good AI test](#22-a-small-example-of-a-good-ai-test)
- [23. What local AI should not replace for me](#23-what-local-ai-should-not-replace-for-me)
- [24. Current practical routine](#24-current-practical-routine)
- [25. A few basic Ollama commands](#25-a-few-basic-ollama-commands)
- [26. What I can still improve](#26-what-i-can-still-improve)
- [27. What this experiment showed me](#27-what-this-experiment-showed-me)
- [28. Closing](#28-closing)

---

## Summary

My workflow is now roughly this:

1. I first think through the question, the project, and the structure myself.
2. I build as concrete a project plan as possible.
3. AI helps me find gaps, technical problems, and possible errors in the plan.
4. Only once the plan is stable do I start on the actual code or SQL.
5. For exact calculations I use Pandas, SQL, or other deterministic tools.
6. Local AI helps with code, debugging, explanations, text, and initial analysis.
7. For difficult or important questions I use multiple models or additionally cloud AI.
8. I check numbers, tables, charts, and statements myself.
9. If something doesn't look right logically or visually, syntactically correct code is not yet proof for me.
10. In the end, I decide myself what goes into a project, a report, or a dashboard.

So I'm not trying to **find one AI I can believe everything from**. I'm trying to have the right tool for each different task.

---

## 1. What this README is – and isn't

This document is a summary of my private tests with local language models via Ollama, as well as my practical work with cloud AI.

It's meant to show:

- why I use local AI,
- why I still use cloud AI as well,
- how I tested different models,
- which models work for which tasks for me,
- where they made mistakes,
- why speed alone isn't enough,
- why bigger models aren't automatically better,
- why a dedicated coding model can still make logic errors,
- how I integrate AI into real data-analysis projects,
- and why the human still has to check things in the end.

It's **not** meant to show:

- that any model is generally "the best model,"
- that my timings would be the same on other hardware,
- that Open WebUI is fundamentally bad,
- that local AI is automatically safe or allowed at every company,
- or that I carried out professional AI benchmarking.

The tests were deliberately practical. I tested models on tasks that actually match my day-to-day work.

---

## 2. My system

The results in this README refer to roughly the following local environment:

| Component | My system |
|---|---|
| OS | EndeavourOS / Arch Linux |
| CPU | AMD Ryzen AI 9 HX 370, 12 cores / 24 threads |
| RAM | ca. 93 GiB usable |
| GPU | integrated AMD Radeon 890M (`gfx1150`) |
| GPU memory | shared memory |
| NVIDIA | none |
| Ollama | local model execution |
| Docker | for additional local services |
| Interface | terminal directly via Ollama; Open WebUI also tested |

This matters. A model that takes 20 minutes on my machine can look completely different on a strong dedicated GPU.

So statements like "model X is slow" in this README are actually shorthand for:

> **Model X was slow at this task, on my system, with my configuration.**

---

## 3. Getting local execution to work properly first

Part of the experiment wasn't the model itself, but the environment.

My integrated AMD GPU wasn't being used meaningfully by Ollama at first. After enabling iGPU usage I could see with `ollama ps` that models were actually running on the GPU.

That check mattered to me. Otherwise you end up comparing models when you're really comparing different hardware paths.

A second issue was context length.

For one model, Open WebUI showed a context of `32768`, even though I wanted to work with smaller settings. A direct request to Ollama with `num_ctx=8192`, on the other hand, correctly ran with 8192.

That was one of the reasons I later ran my actual comparison tests **directly in the terminal**.

---

## 4. Why I currently prefer working directly with Ollama

I initially tested a lot through Open WebUI. Some large models seemed almost unusably slow there: several minutes, sometimes timeouts.

Then I started the same models directly:

```bash
ollama run MODELNAME
```

And suddenly the picture was sometimes completely different.

Some models started responding immediately, streaming word by word. That feels much better to me, because I can see right away that something is happening.

That does **not** mean Open WebUI is bad.

Possible reasons for the difference include, for example:

- longer system prompts,
- chat history,
- tools / function calling,
- extra context data,
- a larger context-window setting,
- my own configuration,
- or things I haven't optimally configured in Open WebUI yet.

I don't want to turn this into a general claim.

For **my current way of working**, though, using Ollama directly is simpler, more transparent, and faster.

Open WebUI can still make sense, especially once I need files, RAG, tools, or a more comfortable interface. For pure model testing, though, I want as few extra layers between prompt and model as possible.

---

## 5. Human, deterministic code, local AI, and cloud AI

I deliberately separate these roles.

| Task | Human | Pandas / SQL / Code | Local AI | Cloud AI |
|---|---:|---:|---:|---:|
| Choosing the research question | **Main role** | – | ideas possible | ideas / discussion |
| Structuring the project | **Main role** | – | support | strong support |
| Exact sums / averages / counts | verify | **main role** | not as a calculation source | not as a calculation source |
| Writing SQL/Python | decide / verify | execute | **very useful** | **very useful** |
| Debugging | verify logically | provide error messages | **very useful** | **very useful** |
| Interpretation | **decisive** | provide facts | first / second analysis | in-depth second opinion |
| Generating charts | verify visually | **main role** | code suggestions | code + formatting |
| Judging charts | **main role** | – | limited | limited |
| Correcting text | final check | – | **very good** | **very good** |
| Documentation | decide on content | – | draft | draft / formatting |
| Current web research | verify sources | – | mostly no | **important** |
| Private local data | decide | local | **advantage** | only if suitable/allowed |

The core idea behind this:

> **Calculation and interpretation are not the same thing.**

If I need an average, Pandas should calculate it. The language model may explain what it might mean. But it shouldn't act as if its verbal answer is itself the data source.

I've implemented this principle in my local data-analysis workflow as well: Pandas handles deterministic calculations; the local model interprets the calculated facts. AI-generated Python code is not executed automatically — it's reviewed first.

---

## 6. Why I want local AI at all

### 6.1 Privacy and habit

Not every file is strictly confidential. Still, I don't want to automatically send everything to an external service.

For some things, my rule is simply:

> If it can stay local, why should it leave my machine at all?

This isn't always a question of "highly sensitive" vs. "not sensitive." It's also about personal habit and data minimization.

Of course: **local doesn't automatically mean allowed**. At a company, internal IT/privacy/compliance rules decide that. If cloud AI is banned, that doesn't automatically mean every local model is allowed.

But I want to be technically capable of working meaningfully without cloud AI.

### 6.2 Cost and limits

Cloud AI is very capable, but long work sessions produce a lot of messages:

- one SQL query,
- an error,
- a new version,
- a new error message,
- checking data,
- correcting text,
- adjusting a chart,
- checking again,
- reworking the README,
- back to the code again.

If every small question has to go to the cloud, a usage or API limit can get burned through unnecessarily fast.

A small syntax question or a normal email doesn't always need to consume the strongest cloud AI, for me.

Local AI is therefore also a kind of **load relief for the cloud**.

### 6.3 Independence

There can be environments where external AI isn't allowed to be used, or is temporarily unavailable.

So I don't want to be in a situation where my entire workflow collapses the moment ChatGPT or another cloud service isn't available.

Local models don't need to be able to do everything. They need to be good enough that I can keep working.

---

## 7. Why cloud AI still plays a very large role

Local AI doesn't fully replace cloud AI for me.

Cloud AI remains especially important for:

- complex, multi-step tasks,
- a strong second or third check,
- current research on the web,
- working with many files,
- extensive formatting,
- finished documents and artifacts,
- difficult methodological discussions,
- longer-term project planning,
- and cases where local models aren't good enough qualitatively.

My goal is therefore not a **local-only system**.

My goal is more:

> **Use local wherever local works well. Use cloud where the extra quality or capability actually adds something.**

---

## 8. My actual project workflow

This has become more important to me than the question of which model is currently "number one."

### Phase 1 – Think it through myself first

Before I let AI generate code, I try to clarify:

- What is my question, actually?
- What do I want to show?
- What data do I have?
- What can I *not* show with this data?
- What metrics do I need?
- Which terms need to be defined cleanly?
- What risks are there for double counting, wrong granularity, or wrong interpretation?

If these things are unclear, AI very quickly produces a lot of code for a poorly defined problem.

That doesn't save time.

### Phase 2 – Turning the idea into a real project plan

After that, I discuss the plan with AI.

A good technical plan includes, for example:

- research question,
- time period,
- data sources,
- tables,
- required columns,
- definition of every metric,
- aggregation level,
- SQL/Python steps,
- query names,
- chart titles,
- expected outputs,
- validation checks,
- known limitations,
- file structure,
- README structure.

This part can save a surprising amount of time.

Once the plan is done, I don't have to rethink, at every small step of the actual project, what a table should be called or what granularity I need.

Because German and English aren't my native languages, a well-prepared plan also helps me stay consistent with:

- titles,
- variable names,
- query names,
- chart labels,
- short explanations,
- README sections.

Then I can carry a lot of it over in a controlled way, instead of redoing the same linguistic groundwork twenty times over.

### Phase 3 – Generate code, but don't run it blindly

AI is very useful to me for a first draft of:

- Python,
- Pandas,
- SQL,
- Dune/Trino SQL,
- data cleaning,
- visualization code,
- functions,
- file exports.

But a code block isn't a result yet.

I look at:

- Which columns are being used?
- Is the granularity correct?
- Is aggregation happening before or after a join?
- Could a merge multiply rows?
- Is `count` being used when `nunique` is meant?
- Is an average being computed at the transaction level or the user level?
- Do the numerator and denominator match up?
- Does the number even match reality?

### Phase 4 – Finding errors faster

Of course there are errors.

Without AI, I can read documentation, search forums, use web search, or check the Pandas/Dune/Trino docs. I still do that too.

But AI can shorten the debugging process a lot. Instead of spending two hours hunting a small syntax or type error, I can often understand where the problem probably lies within a few minutes.

That's one of the biggest practical advantages for me.

### Phase 5 – Look at the result, not just the code

One point matters a lot to me:

> **Code can look plausible and still produce an absurd result.**

A language model often sees a clean code block and explains very convincingly why it's good.

A human looks at the chart and thinks:

> Wait. That can't be right.

For example:

- a share above a mathematically possible limit,
- a chart with a strange scale,
- numbers that contradict each other,
- a token with the wrong label,
- an average that doesn't match the distribution,
- a top-10 share that's obviously implausible.

That's why, for me, **visual review and plausibility checking** are part of the analysis, not just the presentation.

### Phase 6 – A second model or a second method

If something is important, I don't necessarily just ask the same model five more times.

I can:

- use a different local model,
- take cloud AI as a second opinion,
- recompute the number myself,
- build a small test table,
- read documentation,
- write an alternative query.

Multiple independent paths are often better than more confidence in the first result.

And very important:

> **If three models make the same mistake, the mistake doesn't become correct by democracy.**

I didn't pick this idea up from testing AI — I learned it from my history studies: even when many sources, or many historians, agree on the same wrong claim, that agreement doesn't make the claim true.

---

## 9. My local data-analysis workflow

I've already built a general local workflow that tries to implement exactly this separation.

The basic idea:

- load and clean CSV → Pandas
- data types / missing values / duplicates → Pandas
- statistics and correlations → Pandas
- first charts → Matplotlib
- structural questions → local RAG
- interpretation → local Ollama model
- safe natural-language calculation questions → predefined Pandas operations
- code suggestions → local code model
- AI-generated code → **not executed automatically**

For simple natural-language calculation questions, the model only produces a small structured plan. The notebook checks this plan and only executes permitted Pandas operations.

I like this much better than:

```text
User asks something
→ LLM writes arbitrary Python code
→ code gets executed automatically
→ hope everything is correct
```

My goal isn't maximum "agent magic," but a workflow I can understand and control.

---

## 10. How I tested the local models

I didn't want to just adopt online rankings.

A model can be very good on a benchmark and still be annoying for my actual work.

So I used tasks that I actually need.

### Language and everyday tests

Among others:

- a short professional email in German,
- naturally correcting flawed English text,
- explaining an API in simple German,
- summarizing four short bullet points,
- responding in normal everyday English without sounding like a teacher.

### Analysis test

I gave it two hypothetical DeFi lending protocols with different borrow volume, different numbers of borrowers, different position sizes, liquidation volume, and borrower concentration.

The model was supposed to:

- separate observation from interpretation,
- name alternative explanations,
- suggest additional metrics,
- identify problematic metrics,
- not invent causality.

### Coding and debugging tests

This is where it got more interesting.

Among the things I tested:

1. **Pandas duplicate weighting**  
   A `transform("sum")` produces per-borrower totals across multiple transaction rows. The average of these repeated values is then incorrectly computed.

2. **Many-to-many merge / double counting**  
   Multiple borrow rows and multiple liquidation rows for the same borrower are merged directly. This creates a cartesian product.

3. **SQL with no bug**  
   One query was deliberately correct. The model was only supposed to report an error if one actually existed.

4. **Writing code from scratch**  
   The model was supposed to build correct monthly metrics, including top-10 borrower share, from a simple DataFrame structure.

5. **Multiple simultaneous bugs**  
   One test combined a merge explosion, incorrect `count`, wrong averaging level, and liquidation volume.

The **no-bug task** in particular was very helpful. It showed that models don't just miss real bugs — they can also invent bugs that aren't there.

---

## 11. A very important test: `COUNT(*)` was correct

The SQL query first aggregated on:

```text
month + protocol + borrower
```

Then it was grouped on:

```text
month + protocol
```

With that, each row of the CTE already corresponds to exactly one unique borrower for that month and protocol.

Which is why:

```sql
COUNT(*)
```

was completely correct.

Even so, both:

- `qwen3-coder:30b`
- `qwen2.5:32b`

claimed that `COUNT(*)` was wrong and needed to be replaced with `COUNT(DISTINCT borrower)`.

The new SQL would have produced the same value in this case, but the reasoning behind it was wrong.

For me, this was an important finding:

> A model can deliver a "fix" that doesn't break the result, and still have misunderstood the logic.

`qwen3.6:27b`, by contrast, recognized fairly early that the query was correct. The problem: it then kept thinking for a very long time, checking the same thing over and over.

**Being right, but spending 30 minutes thinking about whether something is right, also isn't ideal in everyday use.**

---

## 12. Second important test: many-to-many merge

The example:

- Borrower `0x1` has two borrow rows.
- The same borrower has two liquidation rows.

A direct merge on:

```python
["month", "protocol", "borrower"]
```

produces:

```text
2 borrows × 2 liquidations = 4 rows
```

This multiplies both the borrow and the liquidation values.

### Results by model

`qwen3-coder:30b` recognized the merge error, but then wrote an incomplete fix itself: it correctly aggregated only one side, which could still multiply values.

`qwen2.5:32b` completely missed the central merge error in a later multi-bug test.

`qwen3.6:27b` recognized:

- the many-to-many join,
- the inflation of borrow volume,
- the inflation of liquidation volume,
- `count` instead of `nunique`,
- the wrong averaging level,

and proposed the clean solution:

1. Aggregate borrows separately.
2. Aggregate liquidations separately.
3. Only then join the aggregated tables.

That was qualitatively the best answer – but clearly slower.

---

## 13. Speed: my observed timings

These timings are **not standardized benchmarks**. They are single real runs on my system.

| Test | Model | Single run (n=1) | Result |
|---|---|---:|---|
| SQL "no bug present" | Qwen3-Coder 30B | ca. 1:12 min | fast, but invented a bug |
| SQL "no bug present" | Qwen2.5 32B | ca. 5:54 min | slower, invented the same bug |
| SQL "no bug present" | Qwen3.6 27B | >30 min, aborted | recognized early that the query was correct, but stayed stuck in thinking |
| Rewriting Pandas code | Qwen3-Coder 30B | ca. 36 s | good solution |
| Rewriting Pandas code | Qwen2.5 32B | ca. 4:10 min | usable, but one fragile part |
| Rewriting Pandas code | Qwen3.6 27B | ca. 20:44 min | logically clean, extremely slow |
| Multi-bug Pandas | Qwen3-Coder 30B | ca. 1:44 min | caught several real issues, but introduced new bugs in the fix |
| Multi-bug Pandas | Qwen2.5 32B | ca. 5:35 min | didn't cleanly fix the central merge problem |
| Multi-bug Pandas | Qwen3.6 27B | ca. 18:24 min | best logic and clean fix |
| 4 language/everyday questions | Mistral Small 3.1 | ca. 3:38 min total | linguistically very good |

For me, this shows very clearly:

> **Speed, model size, and accuracy are three different things.**

---

## 14. The models I tested

### `mistral-small3.1`

**Tested for:** German email, English correction, everyday language, simple technical explanation.

**Strengths:** natural language, controlled corrections, friendly without being unnecessarily formal.

**Weakness:** not the absolute fastest small model on my system.

**Role:** **top candidate for everyday use, writing, language, and normal conversation.**

### `gemma2:9b-instruct`

**Strengths:** very fast, good short emails, good simple explanations, good natural English.

**Weaknesses:** issues with tools / function calling in Open WebUI; sometimes changes style more than desired.

**Role:** **very fast everyday and office model.**

### `qwen2.5:14b-instruct`

**Strengths:** fast, factual, good summaries, good email and explanation text.

**Weaknesses:** somewhat stiffer at times; overlaps heavily with other everyday models.

**Role:** **factual general/office model, possibly redundant later.**

### `gpt-oss:20b`

**Strengths:** good natural language, good corrections, pleasant all-rounder.

**Weaknesses:** on technical questions, several confidently stated errors or exaggerations, including on RAM/VRAM, Ollama/GPU, model sizes, and Ethereum history.

**Role:** **good generalist for language and everyday use, but I verify technical claims.**

### `llama3.1:8b-instruct`

**Strengths:** very fast, usable for simple questions, good simple English correction.

**Weaknesses:** weaker on technical precision; language sometimes a bit clunky.

**Role:** **very fast small fallback, but possibly redundant with Gemma.**

### `qwen2.5:32b`

**Strengths:** direct answers, good general analysis, noticeably nicer in the terminal than earlier in Open WebUI.

**Weaknesses:** often clearly slower than Qwen3-Coder in coding comparisons and not better; invented a bug in the correct SQL test; missed the most important structural error in the more complex Pandas test.

**Role:** **general larger analysis, not my primary coding model.**

### `qwen3-coder:30b`

**Strengths:** surprisingly fast on my system, good code drafts, Python and SQL, very practical for daily coding.

**Weaknesses:** not an automatic logic checker. It invented a bug in a correct SQL query, initially fixed a many-to-many bug incompletely, and in a later test even produced code that couldn't have worked as written.

**Role:** **my main model for writing code, quick Python/SQL help, and normal debugging.**

> **Code from the coder model is a draft, not an approval.**

### `qwen3.6:27b`

This was the most interesting model in the test.

**Strengths:** good methodical analysis, separates observation from interpretation better, finds alternative explanations, recognizes aggregation levels, was qualitatively the strongest on difficult Pandas/SQL logic tests.

**Weakness:** **thinking. A lot of thinking.** It can recognize a correct answer relatively early and then keep checking and repeating for minutes afterward. On some tasks, 18, 20, or over 30 minutes.

**Role:** **deep reviewer.** Not for every small question, but for important logic checks.

### `qwen3.5:27b`

Even run directly via Ollama, generation was very slow in my test. No clear advantage over my other models.

**Role:** **deletion candidate.**

### `deepcoder:14b`

Caught a subtle bug early in one Pandas test, but then got stuck in a very long thinking loop and practically never finished.

**Role:** **deletion candidate.**

### `qwen2.5-coder:7b`

**Advantage:** very fast.

**Problem:** didn't cleanly recognize the crucial duplicate-weighting logic and suggested a wrong fix.

**Role:** **deletion candidate.**

### `nomic-embed-text`

Not a chat model. Used for embeddings, semantic search, and local RAG.

**Role:** **keep.**

---

## 15. My current model allocation

This isn't a permanent truth. It's my current state after these tests.

| Role | Model | Why |
|---|---|---|
| Everyday / writing / language | **Mistral Small 3.1** | natural, controlled, good corrections |
| very fast everyday tasks | **Gemma 2 9B** | very fast and good enough |
| writing code / normal debugging | **Qwen3-Coder 30B** | best mix of speed and coding usefulness |
| difficult logic checking | **Qwen3.6 27B** | qualitatively strongest reviewer in my tests |
| general larger analysis | **Qwen2.5 32B** | usable large generalist |
| embeddings / RAG | **nomic-embed-text** | different technical role |

Other models like Qwen2.5 14B, GPT-OSS 20B, or Llama 3.1 8B can still be useful, but overlap more strongly with the roles above.

My clearest deletion candidates after the tests so far are:

- `deepcoder:14b`
- `qwen2.5-coder:7b`
- `qwen3.5:27b`

---

## 16. What I learned from the comparison

### 16.1 Bigger isn't automatically better

A 32B model can be slower and make the same mistake as a 30B model. A smaller model can be more pleasant for a clear everyday task than a much bigger one.

### 16.2 "Coder" doesn't mean "always logically correct"

A coding model can know syntax, libraries, and structures very well and still build an incorrect aggregation. That's especially dangerous in data analysis, because the code can still run without errors.

### 16.3 Code that runs isn't automatically correct code

A query can compile, run, and produce a nice-looking table – and still be methodologically wrong.

### 16.4 Negative tests matter

Don't just ask: "Can you find the bug?"

Sometimes deliberately give it correct code and ask: "Is there a **confirmed** bug?"

That shows whether the model is actually checking, or just wants to invent a bug because the prompt sounds like it's asking for one.

### 16.5 More thinking isn't automatically more truth

Qwen3.6 showed that long thinking phases can produce very good results. But it also showed that a model can repeat the same correct reasoning for a very long time.

> **Thinking is not a quality certificate.**

### 16.6 Streaming changes usability a lot

For me, a model that starts responding immediately and keeps writing slowly is often more pleasant than one that appears to do nothing for five minutes and then delivers everything at once.

### 16.7 Multiple models can share the same reasoning error

Qwen3-Coder and Qwen2.5 32B reached practically the same wrong conclusion on the `COUNT(*)` test.

So "ask a second model" is good, but no guarantee of independent verification.

Sometimes what you need instead of a second LLM is simply math, small test data, documentation, or checking it yourself.

### 16.8 Why I still enjoy reading the thinking output

I enjoy reading a model's thinking output, even when it takes half an hour for a simple question or a code correction.

The reason: you learn a lot from it.

You see:

- contradictions in the model's own reasoning,
- doubts that don't show up anymore in the final answer,
- how the model actually thinks and what logic is behind it,
- how dangerous it would be to just take these intermediate steps as facts.

This isn't just true for technical questions, for me. Especially with very general questions, I find the question and the reasoning behind it extremely interesting: you get a glimpse of how developers think, and you notice where large companies or states might be pushing certain narratives or trying to shift values.

So my recommendation: read the thinking output when a model shows it. It helps you understand how a model actually works – and maybe also helps you not believe everything an AI says.

---

## 17. AI and accuracy

I'm very precise on projects. That can sometimes slow work down, but in data analysis it's also an advantage.

If a number seems off to me, I don't accept it just because:

- the code runs,
- two models say it's correct,
- or the text sounds professional.

I'd rather check again.

In an earlier Solana project, this was exactly what mattered: a value wasn't mathematically plausible. The SQL code was syntactically valid, but the result couldn't have been right.

Another example was token metadata. A label in a data source looked official but was wrong. That had to be checked manually, outside the query.

For me, that's the difference between:

```text
generating code
```

and:

```text
doing analysis
```

AI can do the first one very fast.

The second one still needs judgment.

---

## 18. Charts: an area where the human stays essential

AI can suggest good Matplotlib, Plotly, or Power BI code.

But a chart can be technically correctly generated and still be bad.

I look at things like:

- axis scaling,
- implausible jumps,
- incorrect normalization,
- clipped values,
- categories that don't belong together,
- colors / legend / readability,
- whether the visual message even matches the numbers.

A language model can look at code and say, "That looks correct."

A human can look at the finished picture and immediately notice: "No. Something's wrong here."

That's why final visual review isn't something I hand off to AI.

---

## 19. How I treat AI-generated code

My preferred pattern:

```text
Question / plan
    ↓
AI suggests code
    ↓
I read the code
    ↓
Code runs
    ↓
I check the output
    ↓
Plausibility check
    ↓
for important metrics: second check
    ↓
only then documentation / dashboard
```

Not:

```text
AI writes code
    ↓
runs
    ↓
done
```

---

## 20. Why I still enjoy coding with AI

Because it's fast.

I don't have to know every bit of Pandas syntax by heart to understand:

- which aggregation I need,
- why granularity matters,
- why a merge can be dangerous,
- or why a result looks wrong.

If I know **what I want to achieve analytically**, AI can dramatically speed up the technical implementation.

For me, that's not a weakness of the workflow — it's exactly the value of the tool.

I'd describe my way of working roughly like this:

> I use AI regularly for code, debugging, and documentation. I define the analytical question, check the outputs, and make the final methodological decisions myself.

---

## 21. My practical quality check

Before I accept an important metric, I ask myself:

1. What's the granularity of the raw data?
2. What's the granularity of the desired result?
3. Did I aggregate correctly before the join?
4. Is the join one-to-one, one-to-many, or many-to-many?
5. Could something be double-counted as a result?
6. Does `count` really mean what I'm calling "number of users"?
7. Do I need `nunique`?
8. Is an average per event, per user, per day, or per position?
9. What exactly is the denominator of a share?
10. Can I recompute the result by hand on a mini example?
11. Does the number match the chart?
12. Is an interpretation actually derivable from the data, or merely plausible?
13. Are there alternative explanations?
14. What can the data source not answer at all?

Once these questions are answered, the model brand matters a lot less to me.

---

## 22. A small example of a good AI test

Instead of just:

```text
Is this code correct?
```

better:

```text
Goal:
- total borrow volume
- unique borrowers
- average total borrowed amount per borrower

Identify only confirmed logic errors.
Do not invent problems.
Explain the aggregation level.
Give corrected code only if needed.
```

Even better: build a small test table where I already know the correct result in advance.

That way I'm not just testing whether the model explains nicely.

I'm testing whether it's actually right.

---

## 23. What local AI should not replace for me

Local AI does not replace, for me:

- documentation,
- source criticism,
- my own plausibility checks,
- web research,
- human judgment,
- an employer's data-protection rules,
- professional/subject-matter responsibility,
- or strong cloud models for genuinely difficult tasks.

It's an additional layer in my toolbox.

Nothing more, nothing less.

---

## 24. Current practical routine

### Small email / English correction

```text
Mistral Small 3.1
or
Gemma 2 9B
```

### Small Linux / everyday question

```text
Gemma / Mistral / small generalist
```

### Writing Python or SQL code

```text
Qwen3-Coder 30B
```

### Checking a weird aggregation or important metric

```text
Qwen3.6 27B
```

and, if needed, additionally:

```text
Cloud AI / documentation / own mini test
```

### Larger general analysis

```text
Qwen2.5 32B
or
Cloud AI
```

### RAG / semantic search over local data

```text
nomic-embed-text + local LLM
```

---

## 25. A few basic Ollama commands

Start a model:

```bash
ollama run qwen3-coder:30b
```

Leave a session:

```text
/bye
```

Check loaded models / execution:

```bash
ollama ps
```

Installed models:

```bash
ollama list
```

Stop a model and free up memory:

```bash
ollama stop qwen3.6:27b
```

Ask a single question directly, without switching into chat mode:

```bash
ollama run gemma2:9b "What is a DataFrame? Explain it briefly."
```

Save the answer straight to a file:

```bash
ollama run mistral-small3.1:latest "Write a short summary about Berlin." > berlin.txt
```

Read the file:

```bash
cat berlin.txt
```

See the answer and save it at the same time:

```bash
ollama run mistral-small3.1:latest "Explain briefly what SQL is." | tee sql.txt
```

Hand a code or text file to the AI:

```bash
cat analysis.py | ollama run qwen3-coder:30b "Check this Python code for confirmed bugs."
```

Save a code review straight to a file:

```bash
cat analysis.py | ollama run qwen3-coder:30b "Check the code and explain the bugs." > code_review.txt
```

For me, `ollama ps` was especially useful for seeing which model is loaded, what context it's using, and whether it's running on GPU or CPU.

---

## 26. What I can still improve

This workflow isn't finished.

Honestly: right now, using individual models directly through WebUI or the terminal often gives me better results than my own automated pipeline does. The intended flow – the model produces a small structured plan, the notebook only executes permitted Pandas operations – isn't reliable enough yet.

Possible next steps:

- retest models after updates,
- save more reproducible benchmarks with identical prompts,
- compare context length and quantization more systematically,
- log RAM/GPU utilization,
- configure Open WebUI more cleanly and retest it against running Ollama directly,
- build automated test cases with known correct results,
- organize local models more by role rather than by "one best model."

I worked very intensively on this automated pipeline for days. At some point, though, a classic 80/20 situation showed up: the last percentage of automation would have cost many more hours, which I now prefer to invest elsewhere. So I only work on it occasionally now, not as intensively as at the start.

What actually stuck, and became my real day-to-day workflow, isn't the finished automation — it's the method behind it: deliberately using different models for different tasks, the way this README describes. That sounds more complicated than it is. In practice it's simple: pick the right model — or cloud AI — for the task at hand, and check the results yourself.

I see this as an experimentation environment, not a finished AI system.

---

## 27. What this experiment showed me

I didn't find the one perfect local model.

In the end, that also wasn't the most important result.

I found something more useful: a division of labor. Which model — or cloud AI — is good for what is in sections 15 and 24. In the end, Pandas and SQL do the calculating, and I decide what I believe and what I publish.

For me, that's more realistic than the idea that a single AI has to be able to do everything.

---

## 28. Closing

I use AI a lot.

But "using AI a lot" doesn't mean, for me:

> I ask an AI and just take its answer.

It means more:

> I use various AI tools, code, documentation, and my own checks to get to a result faster – and I check especially carefully wherever an error can easily look professional.

The models save me time.

They don't save me the thinking, though.

And that's exactly the combination I want.

> **Use AI. But use your brain. Please!!!**
