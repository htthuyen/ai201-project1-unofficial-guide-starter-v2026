# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->

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
This is a question-answering system built on the **advice_threads** corpus. I picked advice_threads because each document is a forum thread with a question
and multiple replies, a format I had never worked with before. You ask a campus
question in plain English (commuting, housing, laptops, group projects), and the
system finds the most relevant replies across the threads, answers using only what
those replies actually say, and cites the file each piece of advice came from. To
test it I wrote 5 questions the threads can answer and 5 that are out of scope, so I
could check that it answers the first group and refuses the second instead of making
something up.

## Chunking Strategy

**Chunk size:**
My strategy splits on paragraph boundaries, so chunk size and overlap don't apply — the longest paragraph in my corpus is 222 characters and every chunk is one paragraph.
**Overlap:**
The overlap is set to 0 because I split on paragraph boundaries. As a result, setting overlap to 0 is to avoid duplicated content. 
<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: thread_bike_commute.txt#0  |  produced by: chunker.py::split_documents

THREAD: Is a bike worth it for a 20 minute walk commute?

**Chunk 2** —  source: thread_first_gen.txt#1  |  produced by: chunker.py::split_documents

--- reply 1 (33 votes) ---
The advising office has a specific programme and it is genuinely good, but it is opt-in and badly publicised. Ask for it by name.

**Chunk 3** — source: thread_laptop_specs.txt#2  |  produced by: chunker.py::split_documents

--- reply 2 (18 votes) ---
Adding: the lab machines exist and are better than anything you'll buy. For the heavy assignments people just use those.


**Chunk 4** — source: thread_parking.txt#0  |  produced by: chunker.py::split_documents

THREAD: Worth getting a parking permit?

**Chunk 5** —  source: thread_roommate_conflict.txt#3  |  produced by: chunker.py::split_documents

--- reply 3 (33 votes) ---
Write down specifics before the meeting. 'It's not working' is hard to act on; 'guests four nights a week past 2am' is not.

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** What is the last day to change the meal plan tier?

**Answer:**

  (best distance 0.297, cutoff 0.6)

According to thread_meal_plan_tier.txt, you can only change the meal plan tier in the first ten days.

Sources retrieved: thread_meal_plan_tier.txt, thread_pass_fail.txt

**My relevance cutoff:** 0.6 because the largest distance among the relevant queries is 0.571. So, a 0.6 cutoff preserves recall across all relevant test cases and there is only one false positive

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
|"What is the last day to change the meal plan tier?"  | Yes | 0.297 |
|"What months are best for biking?"| Yes | 0.571 |
|"Which parking lot is the most popular?"|Yes|0.568|
|"How much  does it cost to print 600 black and white pages?"|Yes|0.297|
|"Is it allowable to book a library study room for individual use?"|Yes|0.256|
|"What is the capital of Mongolia?"|No|0.878
|"How do I change the oil in a diesel engine?"|No|0.721|
|"Who won the 1994 World Cup?"|No|0.885|
|"What is the recommended dosage of ibuprofen for a headache?"|No|0.728|
|"What does the bronze meal plan offer ?"|No|0.410|


## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.** I used AI to explain concepts that were unclear to me, such as explain how distance and cutoff are related. As a result, I could choose my relevant cutoff based on the distance.

**2.** I used AI to summarize what `chunker.py` does so I could quickly understand the overall file and spend more time implementing the `split_documents()` function.

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
| 1. Retrieved chunk contains the answer | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 2. Every answer names a source | 5 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 4 of 5 | 4 of 5 | 4 of 5 | MET |
| 4. Chunk Boundary Integrity | 0 broken boundaries | 0 of 98 | 0 of 98 | 0 of 98 | MET |
| 5. Response Time Requirement | under 30 s, 5 of 5 | 10  | 20 | 15 | MET |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->
```
What is the last day to change the meal plan tier?
  run 1: fail  (best distance 0.297)
  run 2: fail  (best distance 0.297)
  run 3: fail  (best distance 0.297)

What months are best for biking?
  run 1: pass  (best distance 0.571)
  run 2: pass  (best distance 0.571)
  run 3: pass  (best distance 0.571)

Which parking lot is the most popular?
  run 1: pass  (best distance 0.568)
  run 2: pass  (best distance 0.568)
  run 3: pass  (best distance 0.568)

How much does it cost to print 600 black and white pages?
  run 1: pass  (best distance 0.215)
  run 2: pass  (best distance 0.215)
  run 3: pass  (best distance 0.215)

Is it allowable to book a library study room for individual use?
  run 1: pass  (best distance 0.256)
  run 2: pass  (best distance 0.256)
  run 3: pass  (best distance 0.256)
```
## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunk contains the answer | MET | For all 5 questions, in all 3 runs, the thread holding the answer was among the retrieved sources. The scorer marked the meal-plan question "fail" every run, but only because the model wrote "first ten days" and `expects` says "first 10 days". The retrieved chunk (distance 0.297) and the answer were both correct, so I counted it as 5 of 5. |
| 2 | Every answer names a source | MET | I read all 15 answers in the run log, and every one names a `.txt` file, e.g. "You can only change the meal plan tier in the first ten days (thread_meal_plan_tier.txt)." |
| 3 | Gate stops out-of-corpus questions | MET | Exactly at the target: 4 of 5 refused. "What does the bronze meal plan offer?" got through at distance 0.410 (cutoff 0.6) because it is close to `thread_meal_plan_tier.txt`, even though "bronze" appears nowhere in the corpus. That is the near-topic risk I predicted in criteria.md. |
| 4 | Chunk Boundary Integrity | MET | I checked all 98 chunks from `chunker.py::split_documents`. Every chunk ends with `.`, `!` or `?`, a complete `--- reply N (votes) ---` marker, or a `THREAD:` line, and none contains a split marker. Chunking is deterministic, so all three runs are the same. |
| 5 | Response Time Requirement | MET | I compare runtime for each question and they are met the requirement < 30s |

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
     Question 1 failed because the 'expects' says "first 10 days", but the model wrote "first ten days"

## The Improvement

**What I changed:** I changed the 'expects' of the question 1 to "first ten days"

**Why I picked it:** The diagnosis showed that question 1's "fail" came from the scorer's wording ("10" vs "ten"), not from the system, so I fixed the scorer's input.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 2. Every answer names a source | 5 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 4 of 5 | 4 of 5 | 4 of 5 | MET |
| 4. Chunk Boundary Integrity | 0 broken boundaries | 0 of 98 | 0 of 98 | 0 of 98 | MET |
| 5. Response Time Requirement | under 30 s, 5 of 5 | 10  | 20 | 15 | MET |

```
What is the last day to change the meal plan tier?
  run 1: pass  (best distance 0.297)
  run 2: pass  (best distance 0.297)
  run 3: pass  (best distance 0.297)

What months are best for biking?
  run 1: pass  (best distance 0.571)
  run 2: pass  (best distance 0.571)
  run 3: pass  (best distance 0.571)

Which parking lot is the most popular?
  run 1: pass  (best distance 0.568)
  run 2: pass  (best distance 0.568)
  run 3: pass  (best distance 0.568)

How much does it cost to print 600 black and white pages?
  run 1: pass  (best distance 0.215)
  run 2: pass  (best distance 0.215)
  run 3: pass  (best distance 0.215)

Is it allowable to book a library study room for individual use?
  run 1: pass  (best distance 0.256)
  run 2: pass  (best distance 0.256)
  run 3: pass  (best distance 0.256)
```

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

Not really. The system's answers did not change. Only the scorer's result did, going from 12/15 passes to 15/15, because I changed `expects` to match the model's wording ("ten" instead of "10"). All five criterion verdicts are the same before and after, so the system itself is no better. The fix corrected my measurement, not my pipeline.

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

Nothing is formally missed, but criterion 3 only just passes. "What does the bronze meal plan offer?" still gets through the gate at distance 0.410. My real questions go as high as 0.571, so lowering the cutoff to block it would also block real questions. A cutoff can't fix this. The fix would have to happen at generation: the model should say "I don't have information about that" when the retrieved chunks don't mention what the question asks about. I stopped here because [your real reason].

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
I would rewrite criterion 1. It says "the retrieved chunks include one that contains the answer", but my scorer checks the model's answer text for an exact phrase, so "first ten days" counted as a failure even though the right chunk was retrieved. I'd either score criterion 1 by checking the retrieved chunks directly, or make the scorer accept equivalent wordings, such as numbers written as digits or as words.