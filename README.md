# Product Management & Design Skills

A collection of skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) and [Claude Desktop](https://claude.ai/download) that help product managers and designers go from idea to execution — write PRDs, explore interface designs, and break specs into actionable tickets.

## Skills

### Write Product Brief

**Trigger:** "write a PRD", "create a product brief", "draft a spec"

Guides you through writing a structured PRD via an interactive interview process. Claude asks targeted questions to understand the problem, explores your codebase for context, and produces a complete brief covering problem statement, user stories, implementation decisions, and testing strategy. The finished PRD is published directly to Linear as a document.

### Design an Interface

**Trigger:** "design an API", "explore interface options", "design it twice"

Based on the "Design It Twice" principle from *A Philosophy of Software Design*. Spawns multiple parallel sub-agents, each constrained to produce a radically different interface design for a given module. Designs are presented side-by-side and compared on simplicity, depth, flexibility, and implementation efficiency — helping you avoid anchoring on your first idea.

### Brief to Tickets

**Trigger:** "break down the PRD", "create tickets from the spec", "turn this into issues"

Takes a PRD stored in Linear and slices it into independently-grabbable Linear issues using vertical (tracer bullet) slices. Each ticket cuts end-to-end through all integration layers rather than splitting by horizontal concern. Issues are created with dependency relationships, acceptance criteria, and references back to the parent PRD.

## Setup

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI **or** [Claude Desktop](https://claude.ai/download)
- A [Linear](https://linear.app) workspace (for PRD publishing and ticket creation)
- Linear MCP server configured in Claude Code or Claude Desktop

### Installation

#### Claude Code

Add this skill set to your Claude Code configuration:

```bash
claude skill add --from https://github.com/ferrants/skills-product-management-design
```

Or clone and add locally:

```bash
git clone https://github.com/ferrants/skills-product-management-design.git
claude skill add --from ./skills-product-management-design
```

#### Claude Desktop

Clone the repository and add each skill's contents to your Claude Desktop [project instructions](https://support.anthropic.com/en/articles/9517075-what-are-projects), or copy individual `SKILL.md` files into your project's custom instructions field.

## Workflow

These skills are designed to chain together naturally:

```
Idea  -->  Write Product Brief  -->  Brief to Tickets  -->  Execution
                                          |
              Design an Interface  <------+
              (when interface decisions
               need deeper exploration)
```

1. Start with **Write Product Brief** to formalize your thinking and publish a PRD to Linear
2. Use **Design an Interface** whenever the PRD surfaces a module that needs careful API design
3. Run **Brief to Tickets** to turn the approved PRD into a prioritized, dependency-ordered backlog in Linear

## License

MIT
