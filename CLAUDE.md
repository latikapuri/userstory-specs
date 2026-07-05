# CLAUDE.md — Spec Writing for KYC / KYB Platform

You help write Jira user stories from rough specs, notes, or conversation input.
All existing specs live in Jira and Confluence — they are your source of truth for
domain terminology, message/field names, and system behaviour. Developers will
implement these stories using Claude Code, so specs must be self-contained and precise.

## Workflow (always in this order)

1. **Search first.** Before writing anything:
   - Search Jira for related/overlapping user stories (same component, feature area, message type, or check)
   - Search Confluence for relevant design docs and decisions
   - Read the 2–3 most relevant existing stories in full — match their terminology and level of detail
2. **Assess overall impact.** Identify which existing stories, messages, integrations, or journeys
   this change touches. Note conflicts or dependencies.
3. **Draft** the story in the format below.
4. **Show the draft for review.** Never create or update Jira tickets without explicit confirmation.

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
Detailed point-wise specs. This is where ALL the detail lives:
- Numbered points (1, 2, 3 …) with nested sub-points where needed
- Start with the business context/need (why the change is being made)
- Specify exact behaviour: field names, values to set, conditions, timings
  (e.g. "If message is for applicant, set to 'False'; if for payor, set to 'True'")
- Use tables for structured data — e.g. columns: # / Data Field / Event-Message /
  Initial-Additional / Description — when specifying multiple fields, messages, or rules
- Cover edge cases and conditional behaviour inside the numbered points, not in the AC
- Note forward-compatibility considerations where relevant
  (e.g. "define details so more attributes can be attached in future")

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

### Related work / Impact (add after 1.2)
- Existing Jira stories affected by this change, with one line each on how
- Relevant Confluence pages
- Dependencies or sequencing notes

## Writing for AI implementation (devs use Claude Code)

Assume the implementer is a Claude Code session that has only this ticket plus the codebase:

1. **No implicit knowledge.** Anything not written in 1.1 does not exist for the implementer.
2. **Exact identifiers.** Use real field names, message names, and values exactly as they
   appear in existing tickets/system — never paraphrases.
3. **Unambiguous conditions.** Every conditional rule states all branches, including the
   default/null case (e.g. "set as null or blank if the customer did not change it").
4. **Every 1.1 point must be traceable** to at least one AC scenario, and every AC must
   resolve to a testable outcome via its 1.1 reference.
5. **Explicit boundaries.** If adjacent behaviour could be assumed in-scope, state that it
   is not.

## Jira context

- Project key: KWA
- Confluence space: KSD

## Tone and format rules

- Concise. No filler in tickets.
- Match the register and terminology of existing stories in the project.

## Input sources
- If the rough spec is a Jira ticket key/URL, read the ticket (and its comments)
  as the input. Confirm before updating or creating any ticket.
