# userstory-specs

Spec-writing workspace for Jira user stories, powered by Claude Code.

Give Claude a rough spec or notes. It searches existing Jira stories and Confluence
docs for related work and terminology, assesses overall impact, and drafts a user
story in our standard format (1.0 Title / 1.1 Conversation / 1.2 Acceptance Criteria).
Nothing is created in Jira without your confirmation.

## Setup (one time)

1. Install Claude Code:
   ```
   npm install -g @anthropic-ai/claude-code
   ```
2. Clone this repo and open Claude Code inside it:
   ```
   git clone git@github.com:<org>/userstory-specs.git
   cd userstory-specs
   claude
   ```
3. Approve the prompt to load the project MCP server (`.mcp.json`).
4. Run `/mcp`, select **atlassian**, and complete the OAuth login in your browser.
   You authenticate with your own Atlassian account — access follows your existing
   Jira/Confluence permissions.

## Daily use

Start `claude` in this folder and give it your rough input, e.g.:

> Rough spec: <paste notes / bullet points / conversation summary>.
> Search Jira and Confluence for related stories, then draft the user story.

Review the draft. When happy:

> Create this in Jira under <EPIC-KEY>.

## How it works

- **CLAUDE.md** — loaded automatically every session. Defines the workflow
  (search first → assess impact → draft → confirm), the user story format,
  and rules for writing specs that devs can implement directly with Claude Code.
- **.mcp.json** — shared Atlassian MCP config (Jira + Confluence access).
  No credentials are stored here; auth is per-person via OAuth.

## Improving the specs

If a spec turned out ambiguous during implementation, or a convention is missing,
update CLAUDE.md via a pull request. That file is the single source of truth for
how our user stories are written.
