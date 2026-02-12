# Init PRD

## Instructions

1. **Output**: This command produces a new `PRD.md` file for the current project. The hierarchy below is the template to follow.
2. **File location**: Create `PRD.md` in the root of the current project. If a `PRD.md` already exists there, rename it first (e.g. `PRD.backup-<timestamp>.md`) so it remains as a backup.
3. **Context gathering**: This command should be invoked at the end of the unstructured vibe-planning phase at the start of a project. Fill the template using information gathered during the current session. For any missing information, ask the user before writing the file. Some sections may be left empty, but only if the user explicitly confirms they should be skipped. Do NOT create the `PRD.md` file until you have full context and all required information.

---

## PRD Template

Below is the full hierarchy. Each section includes a short explanation and an example drawn from a real PRD (an Obsidian AI agent called "Paddy") to illustrate what belongs there.

---

### `# Product Requirements Document: [Product Name]`

Top-level title. Replace `[Product Name]` with the actual project name.

---

### `## Executive Summary`

A short paragraph introducing the product, what it does, who it's for, and how it works at a high level.

#### `**[Product Name]** - [Description]`
One-liner about the product.
> Example: **Paddy** is an AI agent that enables Obsidian users to interact with their vaults using natural language through a self-hosted, Dockerized FastAPI backend.

#### `**MVP Goal:** [Goal description]`
A single sentence describing the minimum viable product objective.
> Example: **MVP Goal:** Enable users to query, read, and manage their Obsidian vaults through conversational natural language, powered by their choice of LLM provider.

---

### `## Mission`

A 1-2 sentence mission statement describing the product's purpose and values.
> Example: Build a production-ready AI agent that makes Obsidian vault management intuitive and efficient through natural language interaction, while maintaining simplicity, transparency, and user control.

#### `### Core Principles`

A numbered list of guiding principles. Each principle has a bold name and a short explanation.

```
1. **[Core Principle 1]** - [Explanation]
2. **[Core Principle 2]** - [Explanation]
...
```

> Example:
> 1. **Self-hosted & Simple** - Easy to set up and run locally
> 2. **Provider-agnostic** - Works with any LLM provider
> 3. **Transparent** - Clear reasoning, visible tool calls, actionable errors

---

### `## Target Users`

#### `**[Target User Group]** who want to: [list]`
Describe the primary audience and list what they want to accomplish.
> Example: **General Obsidian vault users** who want to: interact with their notes using natural language, automate repetitive vault management tasks, ...

#### `**Technical Comfort Level:** [Description]`
Describe the expected technical skill level of the target user.
> Example: **Technical Comfort Level:** Comfortable with basic command-line tools (installing dependencies, setting environment variables, running local services)

---

### `## MVP Scope`

#### `### In Scope`

Break the in-scope items into categorized bold-labeled lists:

- **Core Functionality:** - what the product does for users
- **Technical:** - implementation details, frameworks, architecture
- **Integration:** - third-party tools and protocols
- **Deployment:** - how the product is packaged and run

Each category is a bullet list of features/decisions.

> Example:
> **Core Functionality:**
> - Natural language querying of vault content (semantic search)
> - Reading notes with contextual information (related notes, backlinks)

#### `### Out of Scope (Future Considerations)`

A bullet list of things explicitly not included in the MVP.
> Example: Cloud hosting, multi-user support, database for conversation persistence, mobile support, etc.

---

### `## User Stories`

#### `### Primary User Stories`

Numbered list of user stories in standard format. Include an example usage for each.

```
1. **As a [user role], I want to [action]**, so that [benefit].
   - Example: "[example natural language request]"
```

> Example:
> 1. **As a vault user, I want to find notes using natural language**, so that I can locate information faster than manual search.
>    - Example: "Find my notes about Python from last month"

#### `### Technical User Stories`

Same format, focused on technical/operational needs.
> Example: **As a user, I want clear error messages**, so that I understand what went wrong and how to fix it.

---

### `## Core Architecture & Patterns`

#### `### Architecture`

Describe the chosen architecture and include a directory structure diagram.

> Example: Vertical Slice Architecture (VSA) with a directory tree showing `core/`, `shared/`, `features/` etc.

#### `### Key Patterns`

Numbered bold-labeled patterns used in the codebase. Each has a short description and optionally a code snippet.

```
**1. [Pattern 1 name]**
- [Description]

**2. [Pattern 2 name]**
- [Description]
```

> Example:
> **1. Single Agent Pattern** - One Pydantic AI agent instance at module level, tools register via decorators.
> **2. Tool Registration Flow** - Tools defined in feature modules, imported as side effects in main.py.

---

### `## Tools`

If the product is an agent or has agent-like tools, describe each tool.

#### `### Tool N: [Tool N name]`

Each tool section contains:

- **Purpose:** - what the tool does
- **Operations:** - bullet list of specific operations
- **Key Feature:** - what makes this tool notable

> Example:
> ### Tool 1: `obsidian_query_vault`
> **Purpose:** All discovery, search, and listing operations (read-only)
> **Operations:** Semantic search, list vault structure, find related notes, search by metadata
> **Key Feature:** Token-efficient with `response_format` parameter (concise vs detailed)

---

### `## Technology Stack`

One `###` subsection per tech stack category. List items with bold names and short descriptions.

```
### [Tech Stack Item 1]
- **[Library/Tool]** (version) - [Purpose]

### [Tech Stack Item 2]
...
```

> Example:
> ### Backend
> - **FastAPI** (0.115.0+) - Web framework with OpenAI-compatible endpoints
> - **Pydantic AI** (0.0.14+) - Agent framework with tool orchestration
> - **Python** (3.11+) - Core language

---

### `## Security & Configuration`

#### `### Authentication`

##### `**[Authentication Method]:** [Details]`
Describe the authentication mechanism.
> Example: **Simple API Key Authentication:** Single API key defined in `.env`, validated on all requests, passed via `Authorization: Bearer <API_KEY>` header.

#### `### Deployment Method & Data Access Method`

##### `**[Data Access Mechanism]:**`
Describe how the product accesses external data or resources.
> Example: **Volume Mounting for Vault Security:** Docker volume mounting to provide secure, sandboxed access to the Obsidian vault.

##### `**How It Works:** [Numbered steps]`
Step-by-step explanation of the data access flow.

##### `**Security Benefits:** [list]`
Bullet list of security advantages.

#### `### Configuration Management`

##### `**Environment Variables (`.env`):** [Code block]`
Show the `.env` file structure with comments.

##### `**[Orchestration] Configuration:** [Code block]`
Show the orchestration config (e.g. docker-compose.yml).

##### `**Configuration Class ([Settings Framework]):** [Code block]`
Show the settings/config class.

#### `### Security Scope`

##### `**In Scope:** [list]`
What security measures are included.

##### `**Out of Scope (Keep Simple):** [list]`
What security measures are explicitly excluded.

---

### `## API Specification`

One `###` subsection per endpoint.

#### `### Endpoint N: [Endpoint N path]`

Each endpoint section contains:

- **[API Format]:** - the format standard being followed (if any)
- **Request:** - example request code block
- **Response:** - example response code block
- **Authentication:** - how auth is passed
- **CORS Headers:** - CORS configuration (if applicable)

> Example:
> ### Endpoint 1: `POST /v1/chat/completions`
> **OpenAI-Compatible Format:**
> **Request:** `{ "model": "paddy", "messages": [...], "stream": false }`
> **Response:** `{ "id": "chatcmpl-123", "choices": [...], "usage": {...} }`

---

### `## Success Criteria`

#### `### MVP Success Definition`

Three bold-labeled lists:

- **Functional Requirements:** - what must work
- **Quality Indicators:** - quality bar expectations
- **User Experience:** - UX-level success criteria

> Example:
> **Functional Requirements:** User can start the product with `docker compose up`, all tools execute successfully, error messages are clear and actionable.
> **Quality Indicators:** No critical bugs in core workflows, API responses follow the expected format.
> **User Experience:** Setup takes <15 minutes for technical users, agent responses are relevant.

---

### `## Implementation Phases`

One `###` subsection per phase.

#### `### Phase N: [Phase N name] ([Period])`

Each phase section contains:

- **Goal:** - one-sentence goal
- **Deliverables:** - bullet list of what gets built
- **Validation:** - how to confirm the phase is done

> Example:
> ### Phase 1: Foundation (Week 1-2)
> **Goal:** Basic agent with one working tool
> **Deliverables:** Project scaffolding, Dockerfile, FastAPI app with endpoint, core agent setup, first tool
> **Validation:** User can search vault via natural language in Obsidian

---

### `## Future Considerations (Post-MVP)`

Three bold-labeled lists:

- **Potential Enhancements:** - features to consider after MVP
- **Integration Opportunities:** - third-party integrations to explore
- **Advanced Features:** - ambitious long-term features

> Example:
> **Potential Enhancements:** Conversation history persistence, streaming responses, semantic embeddings
> **Integration Opportunities:** Model Context Protocol (MCP) server, other plugins
> **Advanced Features:** Graph relationship analysis, voice input/output

---

### `## Risks & Mitigations`

One `###` subsection per risk.

#### `### Risk: [Risk N name]`

Each risk section contains:

- **Mitigation:** - how the risk is addressed (paragraph or bullet list)

> Example:
> ### Risk: Vault Corruption
> **Mitigation:** File operations use atomic writes, delete operations require confirmation, validation before destructive operations, users maintain backups.

---

### `## Appendix`

#### `### Related Documents`
Links to related specs, design docs, or external references.

#### `### Key Dependencies`
Links to key libraries, frameworks, and tools used.

#### `### Repository Structure`
A directory tree showing the final project layout.
