# Router Policy

Task classification and model routing rules.

---

## Principles

1. **Cheapest first.** haiku < gemini < openai < sonnet < fable/opus
2. **Direct answer over delegation** - if the task is simple, answer directly
3. **Skill = methodology**, not executor. Skills apply silently on top of the main response
4. **Don't spawn agents without reason** - only when context isolation is needed
5. **Haiku for gate-keeping** - if the task is classification/validation/parsing, Haiku first
6. **Fable for creative** - storytelling, metaphors, unusual formats - not Sonnet

---

## Task Classification (6-tier routing)

| Task type | Executor | Cost tier |
|---|---|---|
| Classification, tagging, format validation, routing, parsing | `haiku-worker` | lowest |
| Summarization, extraction, drafting, broad reading, translation, log analysis | `gemini-worker` | low |
| Focused coding, tests, refactors, patch suggestions | `openai-worker` -> fallback `sonnet-engineer` | low-medium |
| Standard engineering, multi-file changes, debugging, Russian content | `sonnet-engineer` | medium |
| Narrative, storytelling, creative content, unexpected brainstorm angles | `fable-creative` | medium-high |
| Architecture, risky migrations, ambiguous root cause, security | `opus-architect` | high |

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
