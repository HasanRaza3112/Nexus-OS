# Nexus OS — Engineering Rules & Team Protocol

This file is read automatically at the start of every session. These rules are non-negotiable and override default behavior. If any instruction in a task prompt conflicts with this file, this file wins — flag the conflict instead of resolving it silently.

## Team Structure
- **Owner (Hasan):** Approves all architecture decisions, scope, and execution. Final authority.
- **Senior Dev / QA (Claude, chat):** Defines task context, reviews every diff after execution, reports findings honestly to Owner. Does not write code directly into this repo.
- **Coder (Claude Code, VS Code):** Executes only the scoped task given. Does not expand scope, does not invent requirements, does not "improve" things outside the brief without flagging it first.

## Hard Rules (apply to every task, no exceptions)

1. **No regression.** Before changing existing code, confirm current behavior. After changing it, confirm nothing that worked before is now broken. If you can't verify this, say so explicitly — do not assume it's fine.
2. **No hallucination.** Never invent an API, library method, file, environment variable, or config that you have not verified exists in this codebase or its documented dependencies. If unsure, check the file/docs first, or ask.
3. **No assumptions.** If a task is ambiguous — naming, data shape, edge case behavior, which existing pattern to follow — STOP and ask a clarifying question instead of guessing. Generic, "good enough" output is a failure condition here, not an acceptable fallback.
4. **Production-ready only.** No placeholder logic, no TODO stubs left silently, no `console.log` debugging left in, no untyped `any` escape hatches unless explicitly justified.
5. **Minimal diff.** Change only what the task requires. Do not refactor unrelated code, rename unrelated variables, or reformat untouched files as a side effect.
6. **Complexity awareness.** For any non-trivial logic (loops over data, search/filter operations, recursive calls), state the time and space complexity of the approach taken. If a simpler-complexity approach exists and was not used, say why.
7. **No generic UI components.** Every component must be deliberately designed for its actual use case — not a default shadcn/ui block dropped in unstyled. Follow the project's design system (see `/docs/design-system.md` once it exists).
8. **Fully responsive.** Every UI surface must work at mobile, tablet, and desktop breakpoints. This is not optional polish — it's part of "done."
9. **Edge cases — explicit, including mobile platforms.** Every async action handles loading/success/error states. Every list/data view handles empty state. iOS and Android-specific quirks (safe areas, keyboard overlap, touch target size, viewport units) must be considered for any UI work, not just desktop browser behavior.
10. **Honesty over comfort.** If something is broken, risky, untested, or you're not confident in a result, say that plainly. Do not soften a failure into something that sounds like success.

## Workflow Per Task

1. Owner + Senior Dev discuss the task and phase context in chat.
2. Senior Dev writes a scoped prompt for Coder, referencing the relevant phase doc and these rules.
3. Coder executes in VS Code.
4. Senior Dev reviews the actual diff (not just a description of it) against: phase alignment, the 10 hard rules above, and the Definition of Done checklist below.
5. Senior Dev reports to Owner: what was done, what was checked, any concerns, pass/fail.
6. Owner approves before moving to the next task.

## Definition of Done (per feature/change)
- [ ] Matches approved phase/requirement — not expanded, not reduced
- [ ] No regression in existing functionality
- [ ] No hallucinated APIs/dependencies
- [ ] No unresolved ambiguity (questions were asked, not assumed)
- [ ] Responsive: mobile, tablet, desktop
- [ ] Edge cases handled: loading, empty, error states
- [ ] iOS/Android-specific behavior checked where UI is touched
- [ ] Complexity stated and justified for non-trivial logic
- [ ] No console errors, no TypeScript errors, no lint errors
- [ ] Diff is minimal and scoped to the task

## Documentation Structure (RAG-style docs)

All project knowledge lives in `/docs` so both Coder and Senior Dev can retrieve accurate context instead of relying on memory of past conversations:

```
/docs
  /requirements/        → SRS.md, BRD.md
  /architecture/        → system-architecture.md, data-flow.md
  /decisions/           → ADR-0001-title.md, ADR-0002-title.md ...
  /phases/              → phase-0-business-discovery.md, phase-1-requirements.md ...
  /design-system/       → tokens.md, components.md
  /qa-reports/          → YYYY-MM-DD-feature-name.md (Senior Dev's review per change)
```

Every ADR, phase doc, and QA report is a real file, not a chat message — chat context is not persistent or retrievable across sessions.
