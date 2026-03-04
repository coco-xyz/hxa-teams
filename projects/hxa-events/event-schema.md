# HxAEvent — Unified Event Schema

All sources normalize their payloads into `HxAEvent` before entering the router. Sinks receive only `HxAEvent` objects — they never see raw source payloads.

## Schema

```typescript
interface HxAEvent {
  // Identity
  id: string;           // globally unique — "{source}:{source_event_id}"
  source: EventSource;  // origin platform
  type: EventType;      // normalized event type

  // Timing
  occurred_at: string;  // ISO 8601, set by source adapter from original timestamp
  received_at: string;  // ISO 8601, set when event enters the mesh

  // Actor — who triggered the event
  actor: {
    id: string;         // platform-native user ID
    name: string;       // display name
    email?: string;
    url?: string;       // profile URL
  };

  // Subject — what the event is about
  subject: {
    id: string;         // platform-native resource ID
    type: SubjectType;  // "pr" | "issue" | "commit" | "page" | ...
    title: string;      // human-readable summary
    url: string;        // direct link
    repo?: string;      // "{owner}/{repo}" for code events
  };

  // Action detail
  action: string;       // verb: "opened" | "closed" | "merged" | "commented" | ...
  summary: string;      // 1-2 sentence plain-language summary (agent-readable)

  // Raw payload — preserved for agent use; do not route on this directly
  raw: Record<string, unknown>;

  // Routing metadata — populated by router or source adapter
  meta?: {
    labels?: string[];           // tags for routing rules
    priority?: "low" | "normal" | "high";
    target_agents?: string[];    // explicit routing hints from source
  };
}
```

## Enum Values

### `EventSource`

```typescript
type EventSource =
  | "github"
  | "gitlab"
  | "notion"
  | "hxa-connect"  // internal — agent-generated events
  | "custom";      // escape hatch for unlisted sources
```

### `EventType`

```typescript
type EventType =
  // Code review
  | "pr.opened"
  | "pr.closed"
  | "pr.merged"
  | "pr.review_requested"
  | "pr.reviewed"
  | "pr.commented"
  // Issues / work items
  | "issue.opened"
  | "issue.closed"
  | "issue.commented"
  | "issue.assigned"
  // Commits / pushes
  | "push"
  | "commit.commented"
  // CI/CD
  | "pipeline.started"
  | "pipeline.passed"
  | "pipeline.failed"
  // Documents
  | "page.created"
  | "page.updated"
  // Generic fallback
  | "unknown";
```

### `SubjectType`

```typescript
type SubjectType =
  | "pr"
  | "issue"
  | "commit"
  | "push"
  | "pipeline"
  | "page"
  | "comment"
  | "unknown";
```

## Example

A GitHub PR merge produces this `HxAEvent`:

```json
{
  "id": "github:1234567890",
  "source": "github",
  "type": "pr.merged",
  "occurred_at": "2026-03-05T08:42:00Z",
  "received_at": "2026-03-05T08:42:01Z",
  "actor": {
    "id": "boot-coco",
    "name": "Boot",
    "url": "https://github.com/boot-coco"
  },
  "subject": {
    "id": "42",
    "type": "pr",
    "title": "feat: ClawFeed Source Market",
    "url": "https://github.com/coco-xyz/clawfeed/pull/42",
    "repo": "coco-xyz/clawfeed"
  },
  "action": "merged",
  "summary": "Boot merged PR #42 'feat: ClawFeed Source Market' into main on coco-xyz/clawfeed.",
  "raw": { "...": "original GitHub webhook payload" },
  "meta": {
    "labels": ["clawfeed", "merge"],
    "priority": "normal"
  }
}
```

## Design Notes

- **`id` uniqueness**: `{source}:{source_event_id}` prevents duplicates across replayed or retried webhooks. Adapters must deduplicate on this key.
- **`summary` is for agents**: write it as a sentence an agent can act on without parsing `raw`. Source adapters are responsible for generating a useful summary.
- **`raw` is preserved but opaque**: the router and sinks must not depend on `raw` structure. It exists so agents can inspect fine-grained detail when needed.
- **`meta.target_agents`**: optional hint from the source adapter. The router uses this to bypass generic routing rules when the source knows the intended consumer.
