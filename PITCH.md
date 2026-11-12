# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A tool loop that carries the full conversation back to Claude after every tool call, clear descriptions for all nine tools (when to call, what each needs, where its inputs come from), and a tenth tool, fare_rules, that reads the Handbook so Claude can explain why a customer does or does not get something.
Does: Looks up the booking, checks the flight's live status, resolves what policy owes the customer, offers real alternatives and explains the rule behind the answer in the Handbook's words. Refunds and anything out of scope go to a human.
Number: Across the five ticket types, 5 of 5 resolved at about 14.8k input and 0.9k output tokens, about 3 tool calls and about 8.5 seconds per customer. Measured before fare_rules was added, so cost is not yet priced against the $6.90 comparison.

Safety check: Not explicitly tested. The agent does not handle out-of-scope requests, referring them to a human instead.

Next: Integrate MCP tool for reading fare rules, continue testing on all 5 ticket types, and work on out-of-scope handling.

Still broken: Fare rules not being read by Claude, and out-of-scope requests not handled.

Lever: speed

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
