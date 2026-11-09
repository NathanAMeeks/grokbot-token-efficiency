# Grok Bot Token Efficiency

Practical rules for cutting token and usage burn when running long-lived Grok Bot agents (research chats, routines, media agents).
Author: **Nathan A. Meeks**. Adopted October 28, 2026; rules added November 7 and 9, 2026.

## Why
A 7-day Grok Bot usage report (Oct 22–29, 2026) showed:
- Chats: **88%** of usage
- Routines: **12%** (two research routines at ~3% each, ~6% check-ins and other work not tied to a routine)

So the biggest lever is the **long chat context**, not the routines. Every request re-sends the conversation, so stale state, pasted logs and repeated re-derivation are the main costs.

## The rules
1. **One source of truth: `STATUS.md`.** Keep a short state file (under ~60 lines, edited in place, never used as an append-only log). Read it before every check-in and every new topic, and start from it instead of from chat history. It records explicit standing decisions (e.g. "PUSH: on hold, do not ask") so periodic check-ins stop re-asking or misreporting state. We saw check-ins report stale or wrong facts; each one cost usage and then needed a correction run.
2. **Big outputs stay in files; reports are one-liners.** Pipe runs to files and report only the summary (e.g. `passed k/n, time per item, failures`). Use `rg -c`, `jq`, `tail -n 5`. Never `cat` large data files, and read only the changed function of a source file.
3. **Quick checks inline, heavy work in the background at full effort.** One-command checks run in-chat, with no subagent. Heavy research (audits, sweeps, builds, multi-run jobs) still goes to background workers at full effort with **no cap**. Only their reports are kept short, so the research isn't nerfed.
4. **Cap recurring research routines.** Give a daily research routine a budget of about **1% of daily usage**. It checkpoints and resumes, and reports in one line.
5. **Media agents:**
   - Approve stills before you generate video. Video is the expensive step.
   - Keep a locked character and style sheet so you don't re-describe or regenerate them.
   - Keep a `MEDIA_STATUS.md`, the media version of rule 1.

## Added rules (November 7 and 9, 2026)
A usage report on Nov 9, 2026 showed **31.7%** of the weekly limit used in about a day, and **bot-to-bot messaging** was the largest cost: roughly 55% of that day's usage. Code used 47% (29 points of it bot-to-bot) and Audit used 19% (17 points bot-to-bot). Routines used only about 2%. The lever had moved from long chats to agents talking to each other.

6. **Research and optimization only on request.** Approved production work keeps running, but agents don't start experiments on their own.
7. **One owner per task.** Hand work off only for a needed action or a final result. No repeated status requests and no acknowledgement chains.
8. **Read only what changed.** Open only the relevant changed code or log excerpts, and reuse saved benchmarks instead of rerunning them. Keep full proof checks and independent audits.
9. **Audit finished candidates only, once each.** If a fix fails twice, it goes to the human before another build-and-audit round.
10. **One heavy job at a time per agent.** Finish or pause one before starting the next.
11. **Stop at milestones.** Big tasks stop and report at a clear milestone instead of continuing open-ended.
12. **Short bot messages, with data passed by file path.** Messages between agents are half their former length, with no chit-chat. Algorithms, code and data are shared as a file path, never pasted. Use agent messages only for needed context or real emergencies.
13. **One-line status updates,** both to the human and between agents.
14. **Event-driven over polling.** Suppress status-only wake-ups, send informational messages without waking the recipient, and prefer connectors and local scripts over desktop or browser work.

## Supporting research (external, cited)
See [`RESEARCH.md`](RESEARCH.md) for sources: context editing, filtering data before it enters context, subagent cost multipliers, model routing and prompt caching. It also covers why output-compression add-ons were **not** adopted: independent measurements found they raised the billed cost or saved little. Lesson: measure the bill, not a tool's self-reported "tokens saved".

## Results
Nathan reports burning far fewer tokens since adopting these rules. No before/after percentage is claimed here, because there was no controlled measurement. The measured figures are the Oct 22–29 baseline (88/12) and the Nov 9 usage shares above. No result for rules 6–14 has been measured yet.

## Templates
- `templates/STATUS.md`: source-of-truth template
- `templates/MEDIA_STATUS.md`: media agent state template
- `templates/routine_prompt.md`: example budget-capped daily research routine prompt

## License
MIT. See `LICENSE`.
