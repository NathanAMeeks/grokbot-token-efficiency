# Grok Bot Token Efficiency

Practical rules for cutting token and usage burn when running long-lived Grok Bot agents (research chats, routines, media agents).
Author: **Nathan A. Meeks**. Adopted October 28, 2026.

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

## Supporting research (external, cited)
See [`RESEARCH.md`](RESEARCH.md) for sources: context editing, filtering data before it enters context, subagent cost multipliers, model routing and prompt caching. It also covers why output-compression add-ons were **not** adopted: independent measurements found they raised the billed cost or saved little. Lesson: measure the bill, not a tool's self-reported "tokens saved".

## Results
Nathan reports burning far fewer tokens since adopting these rules. No before/after percentage is claimed here, because there was no controlled measurement. The baseline above (88/12 split) is the only measured figure.

## Templates
- `templates/STATUS.md`: source-of-truth template
- `templates/MEDIA_STATUS.md`: media agent state template
- `templates/routine_prompt.md`: example budget-capped daily research routine prompt

## License
MIT. See `LICENSE`.
