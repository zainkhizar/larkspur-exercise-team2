# Client review: Larkspur disruption-care agent

**To:** Room 2  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:41

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md says fare_rules was added as a tenth tool, but agent.py has no fare_rules schema and LOCAL_TOOLS holds three other names.**

The static scan lists 12 schemas: nine shipped plus cause_in_plain_words, seats_left and ground_stop_status. LOCAL_TOOLS maps those same three names. The last wire run in readout-trace.json calls fare_rules as its fourth tool, and no definition for it appears in agent.py or in the diff. PITCH.md also says "Still broken: Fare rules not being read by Claude".

Run python3 run.py --show-tools and paste the full list, with the line for fare_rules or the line showing it is absent.

**2. PITCH.md Number reports 14.8k input tokens, 3 tool calls and 8.5s, and readout-trace.json shows 16870 in, 4 tool calls and 8.6s.**

The committed wire run is 4 API turns, 4 tool calls, 16870 tokens in and 856 out, with 0 cache read and 0 written. PITCH.md states "5 of 5 resolved" and gives averages, and no evals/cases.json exists in the repo. The only trace in the repo is one run, and nothing in it shows how the other four ticket types were counted as resolved.

Run python3 run.py --all --trace and paste the totals footer.

**3. Your lever is speed, and the run you committed carries thinking={"type": "adaptive"} with 0 cached tokens on every turn.**

agent.py has no cache_control anywhere and no streaming, and run_agent() resends runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM and the full tools list on all 4 turns. readout-trace.json shows 16870 tokens in over 4 turns and "hit ratio None". Neither a bigger model nor a smaller one changes any of those three lines, so none of them answers Priya's question with a model name.

Run python3 bench.py --label before --stage 1 --runs 3 and python3 bench.py --compare before after, and paste the compare output.

**4. Your PITCH.md says safety is "Not explicitly tested" while TONE_ADDENDUM in agent.py is still 0 characters.**

The system prompt sent on every turn is runtime_preamble() + SYSTEM_PROMPT + "", so nothing in your build tells the agent what to do with an abusive customer or a legal threat. No evals/cases.json exists. You wrote "out-of-scope requests not handled", and the escalate_to_human description in build_tools() already names groups, partner segments and minors, so an out-of-scope case is one you can run today.

Run python3 verify.py 4.1 and paste the output.

**5. seats_left in agent.py fetches seats_left_economy and seats_left_premium and ignores its cabin argument when choosing which to return.**

The function returns both counts and echoes the request back as asked_cabin, so a customer who asked about premium gets the economy count in the same dict. The ground_stop_status description tells the model to read a stop window such as "13:02-15:50" out of free-text remarks. That window depends on how the remarks are written in flights.csv, and nobody has traced either tool yet. readout-trace.json shows neither tool was called in the committed run.

Run python3 run.py K7PQ2M --trace after a question that forces seats_left or ground_stop_status, and paste the tool_result lines.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (385 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `pod_sync.py`, `run.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
