# Adapter Interface Specification

Adapters are the translation layer between external platforms and the hxa-events mesh. There are two adapter types:

- **Source adapters** — receive platform-specific payloads, emit `HxAEvent`
- **Sink adapters** — receive `HxAEvent`, deliver to a target platform or agent

## Source Adapter

Source adapters sit at the ingress boundary. They validate incoming payloads, normalize them into `HxAEvent`, and pass them to the router.

### Interface

```typescript
interface SourceAdapter {
  /**
   * Adapter identity — used in event ID prefixes and logs.
   * Must be stable across restarts.
   * Example: "github", "gitlab", "notion"
   */
  readonly source: EventSource;

  /**
   * Verify that an incoming request is authentic.
   * For webhook sources: validate HMAC signature, token, etc.
   * Throw or return false if verification fails — the mesh will drop the event.
   */
  verify(request: IncomingRequest): boolean | Promise<boolean>;

  /**
   * Parse the raw request body and extract all events it contains.
   * A single webhook call may produce multiple events (e.g., a push with many commits).
   * Return an empty array if the payload is a no-op (e.g., ping events).
   */
  parse(request: IncomingRequest): HxAEvent[] | Promise<HxAEvent[]>;
}
```

### Supporting Types

```typescript
interface IncomingRequest {
  headers: Record<string, string>;
  body: unknown;           // parsed JSON or raw string
  raw_body?: Buffer;       // needed for HMAC verification
}
```

### Contract Rules

1. **`verify` is always called before `parse`.** If `verify` fails, `parse` is never called.
2. **`parse` must never throw** on malformed input — return `[]` and log the error internally.
3. **Each returned `HxAEvent` must have a unique `id`** within the source. The mesh deduplicates by `id`.
4. **`occurred_at` must come from the original payload**, not the adapter's wall clock.
5. **`summary` must be human-readable and agent-actionable** — avoid raw field dumps.

### Example: GitHub Source Adapter (skeleton)

```typescript
class GitHubSourceAdapter implements SourceAdapter {
  readonly source = "github" as const;

  verify(req: IncomingRequest): boolean {
    const sig = req.headers["x-hub-signature-256"];
    const expected = computeHMAC(process.env.GITHUB_WEBHOOK_SECRET, req.raw_body);
    return timingSafeEqual(sig, expected);
  }

  parse(req: IncomingRequest): HxAEvent[] {
    const event = req.headers["x-github-event"];
    const payload = req.body as Record<string, unknown>;

    if (event === "ping") return [];

    return [normalize(event, payload)];  // normalize() maps GitHub fields → HxAEvent
  }
}
```

---

## Sink Adapter

Sink adapters sit at the egress boundary. They receive a routed `HxAEvent` and deliver it to the target platform or agent in whatever format that platform expects.

### Interface

```typescript
interface SinkAdapter {
  /**
   * Adapter identity — used in routing config and logs.
   * Example: "hxa-connect", "lark", "slack"
   */
  readonly sink: string;

  /**
   * Deliver the event to the target.
   * Return true on success, false on failure.
   * The router may retry on false; throw only for unrecoverable errors.
   */
  deliver(event: HxAEvent, context: DeliveryContext): boolean | Promise<boolean>;
}
```

### Supporting Types

```typescript
interface DeliveryContext {
  /**
   * Routing-supplied destination — platform-specific.
   * Examples:
   *   hxa-connect: "org:default|Jessie"
   *   lark:        "oc_abc123"
   *   slack:       "#clawfeed-events"
   */
  destination: string;

  /**
   * Routing rule that matched — for logging and debugging.
   */
  matched_rule?: string;

  /**
   * Whether to suppress delivery (dry-run mode).
   * Adapter should log what it would have done, then return true.
   */
  dry_run?: boolean;
}
```

### Contract Rules

1. **Sink adapters must be idempotent** — re-delivering an event with the same `id` must not cause duplicate messages. Use `event.id` as an idempotency key where the platform supports it.
2. **Delivery failures are soft** — return `false`, do not throw. The router handles retries and dead-letter logic.
3. **Sink adapters format for their platform** — they own the message template. A Lark sink sends a card; a Slack sink sends a block; an hxa-connect sink sends a structured message.
4. **Agents receive the full `HxAEvent`** — when the sink is an agent (hxa-connect), pass the full event as structured data, not as a formatted string. Agents should parse, not parse text.

### Example: HxA-Connect Sink Adapter (skeleton)

```typescript
class HxAConnectSinkAdapter implements SinkAdapter {
  readonly sink = "hxa-connect";

  async deliver(event: HxAEvent, ctx: DeliveryContext): Promise<boolean> {
    if (ctx.dry_run) {
      console.log(`[dry-run] would send ${event.type} to ${ctx.destination}`);
      return true;
    }

    try {
      await c4send(ctx.destination, JSON.stringify(event));
      return true;
    } catch (err) {
      console.error(`[hxa-connect sink] delivery failed:`, err);
      return false;
    }
  }
}
```

---

## Router Contract

The router is not an adapter, but its contract is defined here for completeness.

```typescript
interface Router {
  /**
   * Given an event, return zero or more delivery targets.
   * Routing logic may be rule-based, agent-driven, or both.
   */
  route(event: HxAEvent): RoutingDecision[];
}

interface RoutingDecision {
  sink: string;                // which sink adapter to use
  destination: string;         // sink-specific target
  matched_rule?: string;       // which rule triggered this
}
```

The router receives events from source adapters, calls `route()`, then calls the appropriate sink adapters' `deliver()` for each decision.

---

## File Layout

```
hxa-events/
  sources/
    github/     GitHubSourceAdapter + helpers
    gitlab/     GitLabSourceAdapter + helpers
    notion/     NotionSourceAdapter + helpers (future)
  sinks/
    hxa-connect/  HxAConnectSinkAdapter
    lark/         LarkSinkAdapter (future)
    slack/        SlackSinkAdapter (future)
  router/
    index.ts      Router implementation
    rules.yaml    Routing rules (agent-readable config)
  schema/
    types.ts      HxAEvent, interfaces, enums (single source of truth)
```
