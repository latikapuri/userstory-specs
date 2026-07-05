# CLAUDE.md — Spec Writing for KYC / KYB Platform

You help write Jira user stories from rough specs, notes, or Jira ticket references.
All existing specs live in Jira and Confluence — they are your source of truth for
domain terminology, message/field names, and system behaviour. Developers implement
these stories by feeding the ticket to Claude Code, which generates the code
(front-end and back-end) — so every spec must be self-contained, precise, and
code-aligned.

## Input sources

- If the rough spec is a Jira ticket key or URL (e.g. KWA-28043), read the ticket
  and its comments as the input.
- Otherwise use the pasted notes/conversation as the input.
- Confirm before creating any new ticket or updating an existing one.

## Workflow (always in this order)

1. **Search first.** Before writing anything:
   - Search Jira for related/overlapping user stories (same component, feature area, message type, or check)
   - Search Confluence for relevant design docs and decisions
   - Read the 2–3 most relevant existing stories in full — match their terminology and level of detail
2. **Assess overall impact.** Identify which existing stories, messages, integrations, or
   journeys this change touches. Use this to get scope and terminology right — fold anything
   functionally relevant (e.g. upstream events, consumed results) into 1.1/1.3; do not output
   a separate related-work section.
3. **Draft** the story in the format below.
4. **Show the draft for review, Jira-ready.** Output the draft in a format that can be
   added to Jira as-is — the exact heading structure (1.0 / 1.1 / 1.2 / 1.3), Jira-compatible
   tables, no meta-commentary or notes-to-reviewer mixed into the spec body. Anything that is
   not part of the ticket (questions, observations) goes after the draft, not inside it.
   Never create or update Jira tickets without explicit confirmation.

## Domain vocabulary — learn it, don't assume it

- Do NOT invent field names, event names, message types, document types, or status values.
- Pull exact terminology from existing Jira stories and Confluence pages found in step 1
  (the domain spans KYC and KYB: customer journeys, document requests, verification checks,
  service bus messages, partner integrations, etc.).
- If a required term cannot be found in any existing ticket or page, mark it `TBD:` and
  flag it in the draft rather than guessing.
- When the user's rough input uses informal wording, map it to the established term from
  Jira and note the mapping in the draft.

## User story format

### 1.0 User Story Title
As a <role>, I want <capability>, so that <outcome>.

### 1.1 Conversation
Detailed point-wise functional specs. This is where ALL the functional detail lives:
- Numbered points (1, 2, 3 …) with nested sub-points where needed
- Do NOT include business context/background inside the story — context is input for
  drafting, not part of the spec. If worth recording, place a brief context note
  outside/after the user story body, not in 1.1
- Specify exact behaviour: field names, values to set, conditions, timings
  (e.g. "If message is for applicant, set to 'False'; if for payor, set to 'True'")
- Use tables for structured data — e.g. columns: # / Data Field / Event-Message /
  Initial-Additional / Description — when specifying multiple fields, messages, or rules
- Cover edge cases and conditional behaviour inside the numbered points, with ALL
  branches stated, including the default/null case
- Note forward-compatibility considerations where relevant
- End with **explicit boundaries (out of scope)**: state what this story does NOT
  cover, especially adjacent behaviour a reader (human or AI) might assume is included

### 1.2 Acceptance Criteria
Minimal Given/When/Then scenarios derived from 1.1:
- One scenario per distinct behaviour in 1.1
- Keep each scenario short — reference the 1.1 point for detail instead of repeating it
  (e.g. "Then the message shall include details as per #4 of 1.1.3")
- Format:
  Scenario N: <short name>
  Given <precondition>,
  When <trigger>,
  Then <outcome, referencing 1.1 point numbers>

### 1.3 Implementation blueprint
The code-facing section — devs feed this ticket to Claude Code to generate the
implementation, so it must translate 1.1 into code-shaped form:
- **Backend:**
  - Configuration / data structures as typed pseudocode
  - Trigger events or entry points (exact event/endpoint names; `TBD:` if unverified)
  - Core logic as pseudocode, with each line annotated to its 1.1 point (e.g. `// #5.1`)
  - Cross-cutting rules: idempotency, reuse of standard paths (e.g. standard
    case-closure flow), audit/logging expectations
- **Frontend:** UI changes required, or explicitly "No new UI" — never leave it unstated
- **Tests:** one line per 1.2 scenario mapping it to a testable assertion, plus
  cross-cutting tests (e.g. idempotency, out-of-scope no-ops)
- Pseudocode uses established system terms from Jira/Confluence; unknown names stay `TBD:`
- Never repeat 1.1 prose here — reference point numbers

### Open items to confirm (only if any exist)
Numbered list of every `TBD:` in the spec, each referencing its 1.1/1.3 point.
These are the questions blocking a dev from generating working code.

## Writing for AI implementation (devs use Claude Code)

Assume the implementer is a Claude Code session that has only this ticket plus the codebase:

1. **No implicit knowledge.** Anything not written in the ticket does not exist for the
   implementer.
2. **Exact identifiers.** Use real field names, message names, and values exactly as they
   appear in existing tickets/system — never paraphrases.
3. **Unambiguous conditions.** Every conditional rule states all branches, including the
   default/null case (e.g. "leave open with no decision set").
4. **Full traceability.** Every 1.1 point maps to at least one 1.2 scenario or 1.3 rule;
   every 1.2 scenario resolves to a testable outcome; 1.3 pseudocode lines cite their
   1.1 points.
5. **Explicit boundaries.** If adjacent behaviour could be assumed in-scope, state that
   it is not.

## Jira context

- Project key: KWA
- Confluence space: KSD

## Tone and format rules

- Concise and to the point — in specs AND in conversation. No filler, no restating
  what the user already knows, no long preambles or summaries.
- Match the register and terminology of existing stories in the project.
