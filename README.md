# Igor Vladyka

I build the layer that lets LLMs act on real systems without acting unsupervised, with an audit trail for every action.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/approval-card-dark.svg">
  <img src="assets/approval-card-light.svg" alt="Approval required. Tool contact_igor. Topic: connecting an LLM to our product. Reads this profile and public repos. Writes one discussion thread. Logs one audit entry." width="520">
</picture>

**[Approve](https://github.com/Igor-Vladyka/Igor-Vladyka/discussions/new?category=general)** opens a discussion thread on this repo. **[Deny](https://github.com/Igor-Vladyka?tab=repositories)** goes to my repositories.

## Adding an LLM to a product that already exists

One tool registry, three doors (chat, external agents, approvals) and one ledger that records every call. The approval gate in the diagram is the card above.

```mermaid
flowchart LR
  U[User] -->|SSE stream| A[Chat agent]
  X[Codex or Cursor] -->|MCP with OAuth2| M[MCP server]
  A --> R[Tool registry]
  M --> R
  R --> Q[Read tools]
  R --> G{Approval gate}
  G -->|approved| W[Write tools]
  Q -.-> L[(Audit ledger)]
  G -.-> L
  W -.-> L
  D[Your docs] --> I[Index with citations]
  I --> Q
  classDef gate stroke:#9a6700,stroke-width:3px
  class G gate
```

## The rules

| Block | The rule |
| --- | --- |
| Tool registry | One definition per tool. Chat and MCP both read from it, so they never drift. |
| MCP server | OAuth2 per user. Never a shared key. |
| Approval gate | Reads run freely. Every write waits for a person, and the wait survives a resume. |
| Audit ledger | Every tool call, approval and denial is recorded once and never edited, with who, what and when. Compliance reads the ledger, not chat logs. |
| Index | An answer points at a file and a passage. A wrong citation fails a test. |

## Also learned

- Tool errors go back to the model as structured results it can recover from, never as stack traces.
- Mask sensitive values before they reach the model provider, not after.
- Stream the answer as it is produced. Buffering hides every failure until the end.

Your system has a different shape? Approve the card above and tell me which block breaks.
