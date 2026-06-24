# Routing Examples

Real-world scenarios showing how the router classifies tasks and selects models.

---

## Example 1: Simple Classification (Haiku tier)

**Input:** "Is this a bug report or a feature request?"

**Route:** `haiku-worker` | Cost: $0.001
**Why:** Binary classification - Haiku handles this instantly at 10x lower cost than Sonnet.

---

## Example 2: Code Summarization (Gemini tier)

**Input:** "Summarize this 50-page document about our API architecture"

**Route:** `gemini-worker` | Cost: low
**Why:** Large context window (1M tokens) at low cost. No deep reasoning needed, just extraction.

---

## Example 3: Bug Fix (Sonnet tier)

**Input:** "The n8n workflow breaks when the webhook payload has nested arrays"

**Route:** `sonnet-engineer` | Cost: $3/$15 per MTok
**Why:** Needs understanding of n8n internals, JSON parsing, and debugging context. Standard engineering.

Router output:
```
DIRECT (apply now):
  1. api-integration-specialist [automation-api] (score: 24.0)
     n8n integrations, webhooks, OAuth, Telegram API
  2. systematic-debugging [code-quality] (score: 12.0)
     Systematic approach to debugging errors
```

---

## Example 4: Creative Content (Fable tier)

**Input:** "Write a story about AI adoption for our newsletter prewarm series"

**Route:** `fable-creative` | Cost: $10/$50 per MTok
**Why:** Narrative and storytelling need Fable's creative capabilities. Sonnet would produce functional but predictable text.

---

## Example 5: Architecture Decision (Opus tier)

**Input:** "Should we migrate from monolith to microservices? We have 50 endpoints and 3 developers."

**Route:** `opus-architect` | Cost: $5/$25 per MTok
**Why:** High-stakes decision with long-term consequences. Needs deep reasoning about tradeoffs.

---

## Example 6: Escalation Chain

**Input:** "Why does our auth middleware randomly return 403 on valid tokens?"

1. **First attempt:** `sonnet-engineer` - analyzes the code, identifies potential race condition
2. **Escalation:** `opus-architect` - confirms root cause is token refresh timing, suggests fix with distributed lock

Router output:
```
DIRECT (apply now):
  1. vulnerability-scanner [review-debug-security] (score: 18.0)
  2. find-bugs [review-debug-security] (score: 15.0)
  3. systematic-debugging [code-quality] (score: 12.0)
```

---

## Example 7: Post-Task Recommendation

After deploying CI/CD:

```bash
$ python skill_router.py --post-task cicd-quick-setup,ci-cd-automation "deployed CI/CD"

NEXT STEPS (related skills):
  1. test-driven-development [code-quality] (score: 12.0)
     TDD - write tests before code
     relation: adjacent (code-quality)
  2. vulnerability-scanner [review-debug-security] (score: 8.0)
     OWASP security audit
     relation: adjacent (review-debug-security)
```
