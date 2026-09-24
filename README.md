# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->
Qiuyan Chen - corpus advice_threads 

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->
I chose the advice_threads corpus, which contains discussion threads where students ask questions and other students reply with advice based on their experiences. The system answers questions about topics such as commuting, laptop recommendations for CS courses, first-generation student resources, and other common campus-life concerns. The documents include multiple replies and sometimes different opinions. It is designed to retrieve the most relevant advice and answer only from those sources.
 

## Chunking Strategy

**Chunk size:** One reply per chunk, plus the original thread question.
**Overlap:** No fixed character overlap. The thread question is repeated in every chunk from the same thread to preserve context.

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->
The advice_threads documents are organized as one question followed by several separate replies. When I read the corpus, I noticed that each reply usually contains one complete piece of advice, so splitting by reply keeps each chunk focused without cutting sentences or ideas in half. I repeat the thread question in each chunk because some replies depend on the original question for context, while a fixed character overlap would be less meaningful for this corpus.


## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. --> 

**Chunk 1** — source: `thread_bike_commute.txt#0` — produced by: `chunker.py::split_documents`

```
THREAD: Is a bike worth it for a 20 minute walk commute?

--- reply 1 (14 votes) ---
Yeah. Cuts an 18 minute walk to about 6. The thing nobody mentions is storage — covered bike parking exists at three buildings and is full by 9am at all three.
```

**Chunk 2** — source: `thread_first_gen.txt#0` — produced by: `chunker.py::split_documents`

```
THREAD: Anything specific for first-generation students?

--- reply 2 (41 votes) ---
The thing I'd say: the unwritten rules are the hard part, not the coursework. Ask about the unwritten rules explicitly. People are happy to explain them and nobody volunteers them.
```

**Chunk 3** — source: `thread_laptop_specs.txt#0 ` — produced by: `chunker.py::split_documents`

```
THREAD: How much laptop do I actually need for CS courses?

--- reply 3 (12 votes) ---
I did two years on an 8GB machine and it was fine until the last project, at which point it very much wasn't. 16 is the answer.
```

**Chunk 4** — source: `thread_parking.txt#1` — produced by: `chunker.py::split_documents`

```
THREAD: Worth getting a parking permit?

--- reply 2 (21 votes) ---
Street parking on Verrill is legal and free and unmarked, which is why half the upper years do it.
```

**Chunk 5** — source: `thread_sleep_schedule.txt#1` — produced by: `chunker.py::split_documents`

```
THREAD: Everyone says fix your sleep. Does it actually matter?

--- reply 2 (37 votes) ---
The library being open until 2am is a trap. It's a resource, not a schedule.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** python app.py --corpus advice_threads ask "Why do some students avoid biking between November and March?"

**Answer:**   (best distance 0.508, cutoff 0.6)

```
```

**My relevance cutoff:**

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
|  |  |  |

In-scope 
1) How much RAM do students recommend for a laptop used in CS courses? (best distance 0.166, cutoff 0.6)

2) How long can a bike reduce an 18-minute walk commute to? (best distance 0.253, cutoff 0.6)

3) Why do some students avoid biking between November and March? (best distance 0.508, cutoff 0.6)

4) What financial support is available for first-generation students for textbooks and travel? (best distance 0.268, cutoff 0.6)

5) What do students recommend doing for heavy CS assignments if a personal laptop is not powerful enough?  (best distance 0.219, cutoff 0.6)

Out of scope 
1) python app.py --corpus advice_threads ask "Who won the 2022 FIFA World Cup?"
(best distance 0.871, cutoff 0.6)
2) python app.py --corpus advice_threads ask "How do volcanoes erupt?"
  (best distance 0.852, cutoff 0.6)
3) python app.py --corpus advice_threads ask "What is the capital of Japan?"
  (best distance 0.890, cutoff 0.6)
4) python app.py --corpus advice_threads ask "How do I bake sourdough bread?"
  (best distance 0.873, cutoff 0.6)
5) python app.py --corpus advice_threads ask "What causes a solar eclipse?"
  (best distance 0.809, cutoff 0.6)

I ran all five in-scope test questions and five clearly out-of-scope questions through the system and recorded the best retrieval distance for each. The five in-scope questions had best distances of 0.1659, 0.2527, 0.508, 0.268, and 0.219, giving a range of 0.1659 to 0.508. The five out-of-scope questions had best distances of 0.871, 0.852, 0.890, 0.873, and 0.809, giving a range of 0.809 to 0.890.

There was a clear gap between the highest in-scope distance, 0.508, and the lowest out-of-scope distance, 0.809. I kept the relevance cutoff at 0.6 because it falls inside this gap. At this cutoff, all five questions covered by the corpus were allowed through the relevance gate, while all five unrelated questions were refused. This suggests that 0.6 separates relevant and unrelated retrieval results well for this corpus.



## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.** I pasted three chunks from my advice_threads corpus into Claude and asked it to identify what question each chunk could answer on its own, or explain what context was missing. Claude pointed out that some replies depended on the original thread question for meaning. Based on that, I changed my chunking strategy so that each reply became its own chunk but the original thread question was repeated at the top of every chunk.

**2.** I pasted my five acceptance criteria into Claude and asked it to explain exactly how each one could be tested using only the wording of the criterion. Claude showed that criteria with counts such as “4 of 5” could be tested directly, while vague wording would be harder to score consistently. I used that feedback to make criteria 4 and 5 more specific, including checking whether sampled chunks preserve complete replies and whether at least 4 of 5 final answers stay supported by the retrieved documents.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks preserve complete ideas | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Answers stay grounded in the corpus | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |

### Criterion 1 — Retrieved chunks contain the answer
File: `results/run_2026-09-23_1936_before.md`  
Function: `run_eval.py::main`
```text
How much RAM do students recommend for a laptop used in CS courses — run 1
Best distance: 0.1496 (passed the gate)
Sources retrieved: thread_first_gen.txt, thread_laptop_specs.txt, thread_pass_fail.txt

Students recommend 16GB of RAM for a laptop used in CS courses (thread_laptop_specs.txt).
```

### Criterion 2 — Every answer names a source
File: `results/run_2026-09-23_1936_before.md`  
Function: `run_eval.py::main`
```text
An emergency fund exists for textbooks and travel that is not means-tested beyond completing a short form.
This information comes from thread_first_gen.txt.
```

### Criterion 3 — Gate stops out-of-corpus questions
File: `results/run_2026-09-23_1936_before.md`  
Function: `run_eval.py::check_out_of_scope`
```text
What is the capital of Mongolia? | 0.893 | refused
How do I change the oil in a diesel engine? | 0.896 | refused
Who won the 1994 World Cup? | 0.893 | refused
What is the recommended dosage of ibuprofen for a headache? | 0.807 | refused
How do I write a for loop in Rust? | 0.835 | refused
```

### Criterion 4 — Chunks preserve complete ideas
File: output from `python app.py --corpus advice_threads chunks`  
Function: `chunker.py::split_documents`
```text
THREAD: Is a bike worth it for a 20 minute walk commute?
--- reply 1 (14 votes) ---
Yeah. Cuts an 18 minute walk to about 6. The thing nobody mentions is storage — covered bike parking exists at three buildings and is full by 9am at all three.
```

### Criterion 5 — Answers stay grounded in the corpus
File: `results/run_2026-09-23_1936_before.md`  
Function: `run_eval.py::main`
```text
For heavy CS assignments, students recommend using the lab machines, which exist and are better than anything you can buy (thread_laptop_specs.txt).
```
<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer | MET | All 5 test questions retrieved a chunk containing the expected answer, which is above the target of 4 of 5. |
| 2 | Every answer names a source | MET | All 5 answers named at least one source document in each of the three runs, meeting the 5 of 5 target. |
| 3 | Gate stops out-of-corpus questions | MET | The relevance gate refused all 5 out-of-scope questions, exceeding the target of 4 of 5. |
| 4 | Chunks preserve complete ideas | MET | At least 4 of the 5 sampled chunks contained a complete reply and thread question without cutting a sentence or reply in half. |
| 5 | Answers stay grounded in the corpus | MET | All 5 test answers were supported by the retrieved documents and did not add unsupported information, exceeding the target of 4 of 5. |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

     I did not miss any of my five acceptance criteria, so there was no specific pipeline failure to diagnose. However, some of my targets were relatively safe because my corpus is small and my test questions were written directly from information I knew was present in the documents.

     If I were to make one criterion stricter, I would tighten Criterion 1. Instead of requiring the answer to appear somewhere in the retrieved chunks for at least 4 of 5 questions, I would require the answer to appear within the top three retrieved chunks for all 5 questions. This would test retrieval quality more directly by checking whether the correct information is ranked near the top instead of simply appearing somewhere in the top five results. 

## The Improvement

**What I changed:** top-k from 5 to 3 in config.py

**Why I picked it:** My first evaluation met all five acceptance criteria, but Criterion 1 was relatively forgiving because retrieval returned five chunks for every question. I decided to reduce top-k from 5 to 3 so that the system provides fewer irrelevant chunks while still trying to retrieve the information needed to answer each question. This tests whether the relevant information is ranked near the top rather than simply appearing somewhere among five results.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks preserve complete ideas | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Answers stay grounded in the corpus | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->
     Yes. Reducing top-k from 5 to 3 kept all five criteria at the same passing results, while making retrieval more focused. For example, the RAM question previously retrieved several unrelated sources, but after the change it returned only thread_laptop_specs.txt, which contained the answer. Because the scores stayed at 5/5 while fewer irrelevant chunks were returned, the change improved retrieval precision without hurting accuracy.


## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->
     None of my original acceptance criteria were still missed after the change. However, the current tests are limited because the five questions were written directly from information known to exist in the corpus. The system could still struggle with differently worded or more ambiguous questions that were not included in this evaluation.


## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
     I would make Criterion 1 stricter. Instead of requiring the answer to appear somewhere in the retrieved chunks for at least 4 of 5 questions, I would require the correct answer to appear within the top three retrieved chunks for all 5 questions. My original criterion was fairly easy to meet with top-k = 5, so the stricter version would better measure whether retrieval ranks the most useful chunks near the top.

