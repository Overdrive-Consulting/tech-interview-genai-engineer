# Evaluation Criteria

Candidates will be evaluated across the following dimensions:

## 1. Technical Implementation (55%)

### LangGraph Orchestration (20%)

- Successfully implements working LangGraph loop (at least one iteration end-to-end)
- Proper node definitions and state management
- Correct edge transitions and conditional routing
- Implements loop control with guardrails (max iterations, budgets)
- State persistence across nodes (sources, notes, draft)

### Composio MCP Integrations (25%)

- **Exa.ai Integration (10%)**

  - Successfully calls Exa search via Composio MCP
  - Retrieves and extracts relevant web content
  - Handles search results and content extraction
  - Manages token limits and content trimming

- **Google Docs Integration (8%)**

  - Creates/updates Google Doc via Composio MCP
  - Formats document with proper sections (Answer, Key Findings, Sources)
  - Includes headings and citations
  - Generates clean, structured output

- **Google Drive Integration (7%)**
  - Places document in specified Drive folder via Composio MCP
  - Creates folder if missing
  - Handles folder permissions and access

### Frontend Interface (10%)

- Functional 1-page UI for query input
- Run button and agent invocation
- Displays step logs and planner decisions
- Shows sources preview
- Returns final Google Doc link
- Clean and usable (no design polish required)

## 2. System Architecture (15%)

### Agent Design (8%)

- Clear separation of nodes and tool wrappers
- Logical graph structure and flow
- Appropriate iteration budget and stopping criteria
- Memory/scratchpad implementation for tracking sources

### Error Handling & Resilience (4%)

- Implements timeouts and retries
- Gracefully handles API failures (rate limits, auth errors)
- Provides meaningful error messages
- Has sensible fallbacks for common failures

### Configuration Management (3%)

- Environment-based config via `.env`
- No secrets in code
- Proper MCP configuration files
- Clean separation of config and logic

## 3. Code Quality (15%)

### Readability and Organization (10%)

- Small, readable modules
- Proper TypeScript typing (where applicable)
- Clear function and variable names
- Logical code organization following suggested structure
- Proper separation of concerns (nodes, tools, API routes)

### Documentation (5%)

- Clear `approach.md` explaining:
  - Graph design decisions
  - Iteration budget rationale
  - Error handling strategy
  - Trade-offs made under time constraints
  - Known limitations and future improvements
- Comments on key implementation choices
- Setup and deployment instructions

## 4. Problem Solving & Communication (15%)

### MVP Choices Under Constraints (5%)

- Focuses on core functionality first
- Makes pragmatic decisions for 3-hour timeline
- Clearly identifies must-haves vs. nice-to-haves
- Implements shallow loop (1-2 iterations) to stay on track

### Failure Mode Analysis (5%)

- Documents potential failure modes:
  - Rate limits (Exa, OpenAI, Google APIs)
  - Timeouts and long-running operations
  - Auth/permission errors
  - Token budget exhaustion
- Explains mitigation strategies implemented

### Technical Communication (5%)

- Clear explanation of design choices
- Discussion of trade-offs
- Notes on what would be improved with more time
- Realistic assessment of limitations
- Responds thoughtfully to questions

## Bonus Points (Optional)

### OAuth Implementation

- End-user OAuth for Google Drive/Docs
- Session storage of tokens
- Logout functionality
- Secure token handling

### Additional Features

- Idempotent writes (update same Doc on re-run)
- Source deduplication (by domain)
- Better source management (limit to 5-8 max)
- Telemetry/logging for debugging
- Improved UI with real-time progress updates
