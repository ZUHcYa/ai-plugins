---
name: "challenge"
description: "Devil's advocate - actively question your approach and assumptions"
---

# /compass:challenge

## Purpose

Takes a deliberately critical perspective on your current work. Identifies hidden assumptions, questions necessity, and proposes simpler alternatives. This is the "hard questions" skill.

## When to Use

- Before starting a large feature (is it worth building?)
- When you're unsure if an approach is right
- After a `/compass:check` shows drift
- When you want a reality check on your current direction

## Workflow

```
User: /compass:challenge
Claude: Devil's Advocate Analysis

        Current work: Adding a caching layer to the API

        Hard Questions:
        1. Do you actually have a performance problem?
           -> No load tests run yet. You're optimizing based on assumption.

        2. What happens if you DON'T build this?
           -> Users might see 200ms slower responses. Is that a real problem
              for your current user count (< 100)?

        3. Is there a simpler solution?
           -> HTTP Cache-Control headers would solve 80% of this with zero code.

        4. What are you NOT building while working on this?
           -> The search feature (session goal) is blocked while you cache.

        5. Who asked for this?
           -> Nobody. This is self-initiated optimization.

        Assumptions Found:
        - "The API is slow" (not measured)
        - "Users will notice the difference" (no user feedback)
        - "We need a custom cache" (standard HTTP caching not evaluated)

        Verdict: PREMATURE OPTIMIZATION
        Recommendation: Shelve this. Return to session goal (search feature).
        Add "Evaluate API performance" to backlog for when you have real data.
```

## What the Skill Does

1. Verify `.compass/` exists. If not: suggest `/compass:init`, stop
2. Read all state files (VISION.md, session.md, rules.md)
3. Analyze current work context:
   - Recent git changes
   - Conversation history (what's being discussed/built)
4. Ask five hard questions:
   - **Necessity:** "Do you actually need this? What evidence supports it?"
   - **Inaction:** "What happens if you DON'T build this?"
   - **Simplicity:** "Is there a simpler way to achieve the same result?"
   - **Opportunity Cost:** "What are you NOT doing while working on this?"
   - **Origin:** "Who asked for this? Is it user-driven or self-initiated?"
5. Surface hidden assumptions (statements treated as facts without evidence)
6. Check against anti-goals in VISION.md
7. Deliver a verdict:
   - Name the anti-pattern if applicable (YAGNI, Scope Creep, etc.)
   - Or confirm the approach is sound
8. Provide a concrete recommendation

## Verdict Categories

| Verdict | Meaning |
|---------|---------|
| SOUND APPROACH | Current work is justified and aligned |
| YAGNI | Building something not needed yet |
| SCOPE CREEP | Expanding beyond the original request |
| OVER-ENGINEERING | Unnecessary complexity or abstraction |
| GOLD PLATING | Polishing beyond usefulness |
| PREMATURE OPT | Optimizing without evidence |
| NIH SYNDROME | Rebuilding existing solutions |
| ANTI-GOAL VIOLATION | Directly conflicts with stated anti-goals |

## Notes

- This skill is intentionally provocative - that's its job
- It does NOT modify any files
- The verdict is advisory, not binding
- If the work is genuinely sound, say so clearly - don't manufacture criticism
- Keep the tone constructive: challenge the approach, not the person
