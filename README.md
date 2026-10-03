# How I Actually Use Local and Cloud AI

**English** | [Deutsch](LOCAL_AI_WORKFLOW_README_DE.md) | [Model tests](MODEL_TESTS.md)

> **Personal practice report – not a general AI benchmark**  
> As of: October 2026

This README describes how I work with AI on my data projects: from the first question to the last check. It applies to my projects, my tools and my way of working.

I am an analyst, not a software developer and not an AI engineer. AI writes most of my code. My work comes before and after: the question, the data, the checking, and the decision about what gets published.

My most important principle:

> **AI can speed up work. It does not take responsibility for whether a result is correct.**

The tests of my local models (timings, errors, which model for which task) are in a separate file: [Model tests](MODEL_TESTS.md).

---

## Table of Contents

- [Summary](#summary)
- [1. Why I work with AI so much](#1-why-i-work-with-ai-so-much)
- [2. Which AI I use for what](#2-which-ai-i-use-for-what)
- [3. Before the project: question, knowledge, sources](#3-before-the-project-question-knowledge-sources)
- [4. Getting and checking the data](#4-getting-and-checking-the-data)
- [5. Analysis: one question at a time](#5-analysis-one-question-at-a-time)
- [6. Charts and notebook](#6-charts-and-notebook)
- [7. The finished notebook is the beginning](#7-the-finished-notebook-is-the-beginning)
- [8. When I stop](#8-when-i-stop)
- [9. What this means for working with me](#9-what-this-means-for-working-with-me)
- [10. Closing](#10-closing)

---

## Summary

1. It starts with a real question of my own.
2. I check whether I understand the topic and whether data and evidence exist.
3. AI suggests sources. I check every source and always decide myself where the data comes from.
4. With the data in front of me, I set the questions again: what can realistically be answered?
5. AI writes the code. I have every result explained to me and ask each time: how can this be checked?
6. I compare numbers and charts with each other.
7. When the notebook is finished, the real checking begins: with a fixed review prompt, several models and several rounds.
8. What I cannot resolve is stated openly in the limitations of the project.

---

## 1. Why I work with AI so much

I do not come from economics or from the blockchain world. I am a historian. But almost every question interests me, and when I have a question, I want to answer it. AI lets me do that as precisely as possible, also in fields that are not my own.

The second reason is personal. I am neurodivergent. My thoughts are faster than my speech. In normal conversations with colleagues this is not noticeable. In test and interview situations, especially at the beginning, I cannot explain my work in detail when speaking. What I say then sounds simpler than the work is.

There is one more point: I rarely use technical terms. This is not because I do not understand the subject. I do not need to know a term to see what is happening in the data. When I do not understand something, I ask for a simpler explanation until I understand it.

AI helps me write down fast thoughts and the missing background, with numbers and charts, so that other people can see what I examined and what I found. This is why my projects are documented in detail. The written form shows my work more precisely than a conversation.

---

## 2. Which AI I use for what

| Tool | Role |
|---|---|
| **Claude** | Main tool for the whole process: planning, code, SQL, debugging, corrections, documentation |
| **ChatGPT** | Searching for sources; final reviewer at the end of a project |
| **Local models (Ollama)** | Connected to VS Code: one model for chat, one for correcting and commenting. Also an occasional code check. I do not use Copilot. |

I use local models not only in VS Code but very often directly in the terminal. This has a big advantage for me: what I try out there with the local AI, I do not have to save or delete. Ctrl+C, the terminal is closed, and everything is gone. I do not have hundreds of saved chats that I must sort and organise. With work this intensive, that is an advantage and not a disadvantage: not every thought has to end up in a file and a folder.

Which local models I tested and how they performed is in the [model tests](MODEL_TESTS.md).

I do not trust any single result, no matter whether it comes from a person or from an AI. This is why I work with several models and check again with code.

### Who does what

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

If I need an average, Pandas should calculate it. The language model may explain what it might mean. But it should not act as if its verbal answer is itself the data source.

I have also implemented this principle in my local data-analysis workflow (see [model tests](MODEL_TESTS.md)): Pandas handles deterministic calculations; the local model interprets the calculated facts. AI-generated Python code is not executed automatically; it is reviewed first.

---

## 3. Before the project: question, knowledge, sources

**The question.** My projects often begin spontaneously: with a news item, with something from social media, with an observation in the Solana ecosystem. They are always my real questions, even if they may sound strange to others.

**Can it be answered?** I think about this, sometimes for half an hour, sometimes for a few days. Then I check two things:

- Do I know enough about the topic to understand what it is about?
- Are there data, evidence and sources for what I want to examine?

**The conversation with the AI.** Only after that do I go to the AI. I describe my idea, my research question and what the result should look like. The AI examines the question, and we discuss it.

**Sources.** I ask the AI where suitable data can be found. It makes suggestions. I look at every source myself and always decide myself where the data comes from.

**Setting the questions again.** Once I know the data, I set the questions one more time. Only now do I see which questions fit the data and which are realistic.

**Summary.** At the end of this phase I ask the AI to summarise everything: research question, data, sources. I check whether it is logical. Only then does the project begin.

Almost everything is fixed in this first phase: which data, which market, which trading venue, which addresses. This takes a long time. In return, getting the data afterwards is fast.

---

## 4. Getting and checking the data

**Getting.** If there is an interface (for example Yahoo Finance, Dune, the Bundesbank), the AI writes the code for it. If there is none, I look for the files myself (CSV, Excel). Everything goes into a fixed folder structure so that nothing gets mixed up.

**Checking.** When the AI tells me the data is fine, that is not enough for me. I run my own check code and look at the result:

- duplicates
- missing values
- missing days
- number of rows and columns
- median and average
- whether the shape of the data matches what I expect

This is general basic knowledge, and I do it in every project.

---

## 5. Analysis: one question at a time

I go through my questions one after the other. Every question gets its own step and its own code.

The same applies to working with the AI itself. If there are several points, the AI should handle one point after the other: only when one point is completely done does the next one come. I have to tell it this several times in every project, which is quite annoying.

At the same time, the AI is a great help exactly here. Usually several questions come up at once. The AI helps me to handle questions that have nothing to do with each other separately, and to see the result together at the end.

For every result I ask the AI the same questions:

- What does this result mean?
- How can one check whether it is correct?

Every point is explained until I understand it. If a number is not plausible, or two checks contradict each other, I continue until it fits together.

The results in my projects therefore did not come from one step. They are the result of many rounds.

---

## 6. Charts and notebook

**Charts.** The charts are mostly made with Matplotlib. I adjust colours and titles myself. Then I compare every chart with the numbers above it: does the picture show the same as the table?

**Notebook.** At the end I give the whole notebook to the AI, with an example of my fixed structure:

- introduction and research question at the start
- load libraries only once
- every step separate
- no repetitions in the code

The AI tidies up. The logic of the code must not change in the process.

---

## 7. The finished notebook is the beginning

Someone who sees a finished notebook thinks: the project is done. For me, the real work starts at this point.

The process at the end is the same for all projects:

1. **Local AI.** Code and notebook go to a local model once more.
2. **Claude with a fixed review prompt.** I have a long, fixed review prompt. It demands: check the numbers, check the logic, find inaccuracies. Every error must be backed by evidence. Invented errors do not count. This runs once or twice, also with different Claude models and thinking levels.
3. **Correction.** The errors found are worked through one after the other.
4. **ChatGPT as final reviewer.** ChatGPT gets the same review prompt with the note that this is the final version. It almost always finds something more. In this role, in my experience, ChatGPT is stronger than Claude.
5. **Cross-check.** ChatGPT's findings go back to Claude with the question: are these errors correct? So far they always were. Claude corrects them.
6. **Checking the correction.** At the end, ChatGPT checks whether the correction is right.

Why ChatGPT only comes at the end: if I gave it a project earlier, there would be too many findings at once, and I would have to explain the whole project again.

**What ChatGPT cannot check.** Sometimes ChatGPT names points it cannot check because data is missing or because a question is unclear to it. Then I ask what exactly it needs. I give it the necessary data when that is possible. But there is data I do not want to give to ChatGPT, and it does not get that. I answer open questions until ChatGPT has checked everything that can be checked.

**What I have to check myself.** Claude and ChatGPT cannot open, read or see everything: for example my dashboards and some links and sources. I check these things myself once more in the last round. If something is inaccurate or wrong, I correct it, or I ask the AI where the error is and how I can correct it.

What is found in the last rounds is mostly small things: an imprecise wording, a rounded number, an old version of a dashboard. I correct them anyway, because I am very exact.

After that I write the README. I discuss the structure with the AI, the text is written step by step, and I correct it.

A project takes many hours this way: sometimes 20, sometimes 50 or more, and for the Telegram project (persian-media-analysis) it was around 100. These are realistic times. No project is one hour of work.

The date of a repository does not show when the work began. A project only goes to GitHub when it is about 90% finished.

There is a simple reason why all repositories were created from September 2026 onwards: in September I started to build my portfolio. For some topics, part of the work had already been done before. In September I then worked day and night to finish the projects one after the other and publish them.

Some projects run in parallel. When the local AI analyses texts for hours, for example, I do not wait. I work on the next project and do small things on the first one in between. For anyone who wonders where the time comes from: every day has 24 hours, and the weekend has two days.

Most of the time, however, I work on one project until it is finished. My focus is better when I have only one task and not several at the same time.

---

## 8. When I stop

AI often says: this is sufficient. For me it usually is not. I keep searching until my question is answered.

There are two reasons why I stop:

- **The data does not exist.** Fees are an example: when a project says that no fee data is available, I tried several queries before, and the table was empty every time.
- **The question makes no sense.** Because many topics are not my field, it can happen that I search for something that does not exist in that form. When I notice this, I stop.

And there is a third case: a large project is finished, and one small thing remains that does not change the result. Then I do not rebuild the whole project. I name the point openly in the limitations.

---

## 9. What this means for working with me

- I work fast, but I check everything several times.
- I prefer to receive tasks in writing.
- I deliver results best in writing: as a report, with numbers and charts.
- If someone wants to know how a result came about, it is in the documentation of the project. I am happy to show it there.

---

## 10. Closing

I use AI a lot. That does not mean I ask an AI and take its answer. It means I work with several models, with code and with my own checks until I believe a result.

The models save me time. They do not save me the thinking.

> **Use AI. But use your brain.**

---

## Rights

© 2026 Amirhoushang Rahmannejad. All rights reserved. You are welcome to read and review this project. Copying, modifying or redistributing it requires my written permission.
