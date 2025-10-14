# Technical Interview – LangGraph Deep Research Agent

## Project Context

Build a small "deep research" agent that, given a user question, (1) researches on the web via Exa.ai (through Composio MCP), (2) drafts a structured answer, and (3) writes the result into a Google Doc saved in a Google Drive folder (both via Composio MCP). Provide a 1-page frontend (Vercel preferred) to run the agent.

**Bonus (preferred):** end-user OAuth so users can connect their own Google Drive/Docs.

## Challenge (3 hours)

Deliver an agent that:

1. **Accepts a natural-language query** from the user.

2. **Orchestrates a research loop with LangGraph:**

   - Use Exa.ai via Composio MCP to search → fetch → extract key passages/links.
   - Plan → read → synthesize → cite.

3. **Creates/updates a Google Doc** and saves it to a target Drive folder via Composio MCP (you can create any folder).

4. **Shows status + final link** to the Doc in a minimal 1-page UI.

## Technical Requirements

### Orchestration (LangGraph)

- **Nodes:** planner → search (Exa MCP) → read/summarize → draft → save_to_docs (Google Docs MCP) → file_to_drive (Drive MCP) → done.
- **Control:** loop with guardrails (max iterations, token/latency budgets).
- **Memory:** short-term scratchpad for sources/citations.

### Integrations (via Composio MCP servers)

- **Exa.ai MCP:** web search + content extraction.
- **Google Docs MCP:** create/update document with sections, headings, citations, and source list.
- **Google Drive MCP:** ensure document is placed into specified folder (create folder if missing).
- Keep credentials/config in `.env` and MCP config files.

### Frontend (1 page)

- **Inputs:** query text, optional folder name/ID.
- **Controls:** Run button, show step logs (planner decisions), sources preview, link to final Doc.
- **Deploy on Vercel** (or anywhere public). No need for design polish, just clean and usable.

### Bonus (Preferred)

- OAuth so users can authenticate their own Google and write to their own Drive/Docs.
- Basic session storage of tokens; logout.

## Suggested Project Structure

```
/
├─ app/ (or pages/)      # Next.js/Vercel UI (single page)
├─ src/agent/
│  ├─ graph.ts           # LangGraph definition
│  ├─ nodes/
│  │  ├─ planner.ts
│  │  ├─ exa_search.ts   # calls Exa via MCP tool
│  │  ├─ summarize.ts
│  │  ├─ draft.ts
│  │  └─ save_to_google.ts # Docs + Drive via MCP tools
│  └─ types.ts
├─ mcp/
│  ├─ composio.exa.config.json
│  ├─ composio.gdocs.config.json
│  └─ composio.gdrive.config.json
├─ server/
│  └─ api.ts             # minimal API route to invoke agent
├─ .env.template
├─ approach.md
└─ README.md
```

## Environment & Config

Add to `.env.template` (fill what you actually need):

```env
OPENAI_API_KEY=
COMPOSIO_API_KEY=
EXA_API_KEY=
GOOGLE_CLIENT_ID=           # bonus OAuth only
GOOGLE_CLIENT_SECRET=       # bonus OAuth only
NEXT_PUBLIC_BASE_URL=       # your deployed URL
DEFAULT_GDRIVE_FOLDER_ID=   # optional; can create if missing
```

### Minimal MCP Tool Call Shape (pseudocode)

```typescript
// exa_search.ts
// Use Composio MCP client to call Exa tool: exa.search, exa.getContents
// Return [{url, title, snippet, content}] trimmed with token limits

// save_to_google.ts
// Use Docs MCP: docs.create or docs.update
// Use Drive MCP: drive.createFolderIfMissing + move file to folder
```

### LangGraph Sketch (pseudocode)

```typescript
// graph.ts
const graph = new Graph({
  nodes: { planner, exaSearch, summarize, draft, saveToGoogle, done },
  edges: [
    ['planner', 'exaSearch'],
    ['exaSearch', 'summarize'],
    ['summarize', 'draft'],
    // loop back if not confident and under iteration budget
    [['draft', 'needs_more'], 'exaSearch'],
    [['draft', 'ready'], 'saveToGoogle'],
    ['saveToGoogle', 'done']
  ],
  state: { question, sources[], notes, draft, docUrl }
})
```

## Getting Started

1. Fork the repo (or create one).
2. Populate `.env` from `.env.template`.
3. Configure Composio MCP servers for Exa, Google Docs, Google Drive.
4. Implement graph nodes incrementally:
   - Hardcode folder or create if missing
   - Create Doc with a title like `Research - <slug(question)>`
   - Append sections: Answer, Key Findings, Sources
5. Build the 1-page UI (Next.js). Add a button to invoke `/api/run-agent`.
6. Deploy to Vercel. Post the link.

## Expected Deliverables

1. **Live link** to the 1-page app (Vercel preferred).

2. **GitHub repo** with:

   - `README.md` (setup + run + deploy)
   - `approach.md` (graph design, iteration budget, error handling, future work)
   - Source code as per structure above

3. **A run that generates a Google Doc link** placed in the specified Drive folder.

## Acceptance Checks (quick)

1. Input a query, e.g., _"What are the main drivers of Alberta's 2024–2025 SME hiring trends? Cite sources."_
2. App shows step logs; completes within reasonable time.
3. Returns Google Doc URL containing:
   - Title
   - Answer (2–5 paragraphs)
   - Bullet Key Findings
   - Sources list with URLs
4. Doc exists in target Drive folder.

## Rules and Guidelines

- You may use anything that helps you complete the task
- Feel free to use AI tools to assist your development
- You can ask clarifying questions at any time
- Focus on core functionality first
- Document any assumptions you make
- Time management is crucial - prioritize MVP features
- Keep the loop shallow (1–2 iterations) to stay within time constraints
- Do your best. If you don't finish, push the progress that you've made and explain how you would go about completing the project.

Good luck with the challenge! We're excited to see your solution.
