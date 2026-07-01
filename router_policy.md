# Router Policy

Task classification and model routing rules.

---

## Principles

1. **Cheapest first.** haiku < gemini < openai < sonnet-4.6 < sonnet-5 < opus < fable
2. **Direct answer over delegation** - if the task is simple, answer directly
3. **Skill = methodology**, not executor. Skills apply silently on top of the main response
4. **Don't spawn agents without reason** - only when context isolation is needed
5. **Haiku for gate-keeping** - if the task is classification/validation/parsing, Haiku first
6. **Sonnet 4.6 vs Sonnet 5:** single-step no planning = `sonnet-engineer` (4.6); multi-step with tools/planning = `sonnet-agent` (5)
7. **Fable only after Sonnet fails** - Fable costs more than Opus, try Sonnet first
8. **Opus = risks only** - security, irreversible prod ops, adversarial review; Sonnet 5 covers architecture and complex debug

---

## Task Classification (7-tier routing - Claude 5 family)

| Task type | Executor | Cost tier |
|---|---|---|
| Classification, tagging, format validation, routing, parsing | `haiku-worker` | lowest |
| Summarization, extraction, drafting, broad reading, translation, log analysis | `gemini-worker` | low |
| Focused coding, tests, refactors, patch suggestions | `openai-worker` -> fallback `sonnet-engineer` | low-medium |
| Quick single-step tasks, RU content, fast edits, dialog | `sonnet-engineer` (Sonnet 4.6) | medium |
| Agentic/multi-step, planning, tool use, long context, complex debug, architecture | `sonnet-agent` (Sonnet 5) | medium-high |
| Narrative, storytelling, creative - ONLY after Sonnet gave flat result | `fable-creative` | very_high |
| Security audit, irreversible prod ops, adversarial review | `opus-architect` | high |

## Escalation Policy

1. Start from cheapest viable route
2. For gate-keeping tasks (classify, validate, parse) -> `haiku-worker` first
3. If external worker returns UNAVAILABLE or ERROR -> escalate to `sonnet-engineer`
4. If `sonnet-engineer` is uncertain or task involves risk -> escalate to `opus-architect`
5. For creative/narrative tasks -> `fable-creative` (not Sonnet)
6. Escalate only once per level. If still unresolved, explain what is uncertain

---

## Task Categories

### 0. Gate-keeping / Classification
**Signals:** sort, classify, tag, validate format, choose from N options
**Route:** haiku-worker

### 1. Dialog / Q&A
**Signals:** question, discussion, explanation, advice
**Route:** direct answer in main session

### 2. Content / Marketing
**Signals:** post, landing, headline, copywriting
**Route:** sonnet-engineer (standard) or fable-creative (narrative/storytelling)

### 3. Sales / Scripts / Messages
**Signals:** sales script, follow-up, client message, objections
**Route:** sonnet-engineer

### 4. Idea -> Product
**Signals:** "build", "create", no clear spec, creative brief
**Route:** brainstorming -> then coding with openai-worker

### 5. Coding / Implementation
**Signals:** specific task, file, function, bug fix, known stack
**Route:** openai-worker -> fallback sonnet-engineer

### 6. Debugging
**Signals:** error, traceback, "doesn't work", "why"
**Route:** sonnet-engineer

### 7. Automation / API
**Signals:** n8n, webhook, workflow, integration, API
**Route:** sonnet-engineer

### 8. Research / Synthesis
**Signals:** "research", "find", "compare", "what is", large document
**Route:** gemini-worker (cheap, large context)

### 9. Architecture / Planning
**Signals:** "how to build", "choose stack", "design", multi-week project
**Route:** opus-architect (if risks/uncertainty)

### 10. Tool Setup / Troubleshooting
**Signals:** MCP, settings, hooks, server config
**Route:** sonnet-engineer in main session

### 11. Creative / Narrative
**Signals:** story, metaphor, analogy, storytelling, unusual format, wild brainstorm
**Route:** fable-creative
