# Example daily research routine prompt (budget-capped)

Run daily at <time>. Budget: about 1% of daily usage. Stop when it's reached.
1. Read STATUS.md and the checkpoint file. Don't rely on chat history.
2. Resume from the checkpoint. Write all large output to files in <work dir>.
3. Update the checkpoint and the relevant STATUS.md line in place.
4. Report ONE line: `range covered, passed k/n, failures (if any)`.
5. If an item fails, stop there. Don't skip it. Report it, and don't send, post or push anything.
