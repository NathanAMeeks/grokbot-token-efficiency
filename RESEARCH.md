# Research notes (compiled Oct 28, 2026)
Figures come only from the cited source. None of them is a prediction for a Grok Bot bill.

| Technique | Evidence | Source |
|---|---|---|
| Keep live context short (context editing, fresh sessions, state file) | Context editing cut tokens 84% in a 100-turn eval | https://claude.com/resources/articles/context-management |
| Filter big outputs before the model sees them | 150,000 → 2,000 tokens (98.7%) | https://www.anthropic.com/engineering/code-execution-with-mcp |
| Structured notes / progress files | qualitative | https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents |
| Use subagents sparingly | agents ~4× chat tokens; multi-agent ~15× | https://www.anthropic.com/engineering/multi-agent-research-system |
| Scale effort/model to the step | RouteLLM: >85% cost cut, 95% of GPT-4 quality on MT Bench | https://www.lmsys.org/blog/2024-07-01-routellm/ |
| Keep the prompt prefix stable (caching) | cache reads 0.1× input price | https://www.anthropic.com/news/prompt-caching |

## Not adopted (measured not to save money)
- rtk: +7.6% cost at low effort in the JetBrains paired benchmark. https://blog.jetbrains.com/ai/2026/07/rtk-claude-code-token-savings/
- "Caveman" terse speech: only −8.5% output tokens. https://blog.jetbrains.com/ai/2026/07/speak-to-ai-agents-like-cavemen-tosave-tokens/
- PointFive study: tool-output tokens −38.4%, but billed cost +6.8%. https://www.pointfive.co/research/token-reduction-is-not-cost-reduction
- LLMLingua: lossy, so it's unsafe for exact digits, code and proofs. https://github.com/microsoft/LLMLingua
