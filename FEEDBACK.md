# Client review: Larkspur disruption-care agent

**To:** Room 2  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:42

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

**1. PITCH.md names a tenth tool, fare_rules, but agent.py defines no fare_rules schema, executor or LOCAL_TOOLS entry.**

PITCH.md Built says "a tenth tool, fare_rules, that reads the Handbook". The static scan finds 12 tool schemas, and EXTRA_TOOLS holds cause_in_plain_words, seats_left and ground_stop_status. LOCAL_TOOLS has the same three keys. The names fare_rules and Handbook appear nowhere in agent.py.

Run python3 run.py --show-tools and paste the full tool list so fare_rules can be found by name.

**2. The PITCH.md Number line (14.8k in, 0.9k out, 8.5s) has no run under it that matches, and the one committed wire run differs.**

readout-trace.json records 4 API turns, 4 tool calls, 16037 in, 1010 out and 10.6s, with no cache read or write. PITCH.md claims "about 3 tool calls" and "5 of 5 resolved" across five ticket types. No evals/cases.json exists, so 5 of 5 has no case list behind it. The line also says it was measured before fare_rules was added, and agent.py has no fare_rules to compare against.

Run python3 run.py --all --trace and paste the totals footer, then run python3 bench.py --label before --stage 1 --runs 3.

**3. Three tools you added, seats_left, ground_stop_status and cause_in_plain_words, are absent from PITCH.md, and the committed run shows one of them fired.**

The readout-trace.json call order is lookup_booking, get_flight_status, cause_in_plain_words, check_policy. seats_left and ground_stop_status never appear in it. The tool descriptions run 377, 463 and 436 characters, so every turn carries them whether used or not. PITCH.md Built does not mention any of the three.

Run python3 run.py --tool-tax and paste the output with the per-tool token cost for these three schemas.

**4. Every API turn in run_agent re-sends runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM and all of tool_list(), with no cache_control anywhere.**

The static scan finds no cache_control, and readout-trace.json shows 0 read, 0 written, hit ratio None. At 16037 input tokens over 4 turns, the system prompt and 12 schemas go over the wire each time. A bigger model would be billed for the same repeated prefix at a higher rate. Nobody has measured what that prefix costs yet.

Run python3 bench.py --label before --stage 1 --runs 3 and paste the input-token and cache-read totals.

**5. seats_left(flight_no, date, cabin) in agent.py accepts cabin but never uses it to pick which seat count to return.**

The function returns seats_left_economy and seats_left_premium together and only echoes the cabin as asked_cabin. Claude must choose the right field from the dict itself. Model size cannot fix a result that leaves the filtering to the caller. The ground_stop_status description also tells Claude to read the stop window from free-text remarks, so the end time depends on how flights.csv phrases it.

Run python3 run.py <PNR> --trace for a booking where seats_left fires and paste the tool_result and the final customer text.

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
