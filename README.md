# aitrips MCP Server

[aitrips.io](https://www.aitrips.io/?utm_source=github&utm_medium=mcp-directory) is a
map-first trip planner. Every stop has coordinates, every leg between stops has a real
route, and the plan is one shared thing rather than a document that goes stale.

This MCP server gives Claude, ChatGPT, Gemini or any MCP-compatible assistant 37 tools
against that plan: create a trip, fill in the days, add transport across 11 modes and have
the route drawn between both ends, then review the whole thing for what's missing.
Everything the assistant writes appears in the browser immediately, and everything you do
in the browser is visible to the assistant.

There is nothing to install. The server is hosted at
`https://www.aitrips.io/api/ai/mcp` and speaks Streamable HTTP.

## Setup

1. Sign up at [aitrips.io](https://www.aitrips.io/?utm_source=github&utm_medium=mcp-directory)
2. Go to **Settings** to generate your API key
3. Add the configuration below to your MCP client

Clients that support OAuth need no key at all. The server implements OAuth 2.1 with PKCE
and dynamic client registration, so it registers itself and asks you to sign in.

### Claude Desktop

Add to your Claude Desktop config (`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS):

```json
{
  "mcpServers": {
    "aitrips": {
      "url": "https://www.aitrips.io/api/ai/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

### Claude Code

```sh
claude mcp add aitrips --transport http https://www.aitrips.io/api/ai/mcp \
  -h "Authorization: Bearer YOUR_API_KEY"
```

### Other MCP clients

Use the Streamable HTTP endpoint `https://www.aitrips.io/api/ai/mcp` with an
`Authorization: Bearer YOUR_API_KEY` header. Full setup notes, including the OAuth flow,
are at [aitrips.io/docs](https://www.aitrips.io/docs?utm_source=github&utm_medium=mcp-directory).

## Try it with

```
plan me three days in kyoto and put the walking routes on the map
add the 9am train from paddington to oxford on the 14th
what's missing from my tokyo trip?
```

The second one is the one worth trying first. Both ends get geocoded and a real route is
drawn between them, rather than a straight line across the map.

## Tools

37 tools. A further 12 admin-only tools exist for platform metrics and are registered only
for an admin role, so they never appear in your `tools/list`.

### Trips

| Tool          | Description                                                          |
| ------------- | -------------------------------------------------------------------- |
| `trip_create` | Create a trip with destination, dates, budget and timezone           |
| `trip_get`    | Get full trip details including items, locations and collaborators   |
| `trip_list`   | List your trips, with an optional status filter                      |
| `trip_update` | Update a trip's name, dates, budget or status                        |
| `trip_delete` | Delete a trip and everything attached to it                          |
| `trip_clone`  | Copy a trip, shifting every date relative to a new start date        |

### Itinerary items

| Tool                         | Description                                                                  |
| ---------------------------- | ---------------------------------------------------------------------------- |
| `trip_item_add`              | Add an activity, accommodation, flight or meal with time, place and cost     |
| `trip_item_list`             | List items, filtered by type, status or date range                           |
| `trip_item_update`           | Update an item's details, status or booking info                             |
| `trip_item_delete`           | Remove an item from a trip                                                   |
| `trip_transport_add`         | Add a leg across 11 modes; both ends are geocoded and the route is drawn     |
| `trip_item_repair_transport` | Recover a missing origin by parsing the item name and geocoding it           |

### Trip review

| Tool          | Description                                                                                                            |
| ------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `trip_review` | Read the whole trip back and report gaps: missing transport, empty days, stale routes, stops with no coordinates, unbooked items close to departure |

### Locations

| Tool                   | Description                                        |
| ---------------------- | -------------------------------------------------- |
| `trip_location_add`    | Save a place with coordinates and a type           |
| `trip_location_list`   | List saved places, filtered by type, city or country |
| `trip_location_nearby` | Find saved places within a radius of a point       |
| `trip_location_delete` | Delete a saved place                               |

### Tasks and reminders

| Tool                 | Description                          |
| -------------------- | ------------------------------------ |
| `trip_task_create`   | Create a task with priority and due date |
| `trip_task_list`     | List tasks, filtered by status       |
| `trip_task_update`   | Update a task's details              |
| `trip_task_complete` | Mark a task done                     |
| `trip_task_delete`   | Delete a task                        |

### Notes

| Tool               | Description                                        |
| ------------------ | -------------------------------------------------- |
| `trip_note_create` | Write a markdown note, optionally pinned to a date |
| `trip_note_list`   | List notes, with an optional date filter           |
| `trip_note_update` | Edit a note                                        |
| `trip_note_delete` | Delete a note                                      |

### Sharing and collaboration

| Tool                        | Description                                            |
| --------------------------- | ------------------------------------------------------ |
| `trip_share_create`         | Create a share link at Viewer or Editor level          |
| `trip_share_list`           | List a trip's share links                              |
| `trip_share_revoke`         | Revoke a share link                                    |
| `trip_collaborators_list`   | List who has access to a trip and at what level        |
| `trip_collaborators_remove` | Remove someone's access                                |

### Discussion

| Tool                     | Description                        |
| ------------------------ | ---------------------------------- |
| `trip_item_comment_add`  | Comment on an item, with @mentions |
| `trip_item_comment_list` | Read an item's comment thread      |
| `trip_item_vote`         | Vote an idea up or down            |

### History

| Tool                 | Description                                       |
| -------------------- | ------------------------------------------------- |
| `trip_activity_list` | Read the activity log: who changed what, and when |

### Search

| Tool     | Description                                 |
| -------- | ------------------------------------------- |
| `search` | Search across trips, items and saved places |
| `fetch`  | Read the full record behind a search result |

`search` and `fetch` carry those exact names because ChatGPT's Deep Research mode requires
them.

## UI resources

Four MCP Apps resources under `ui://aitrips/*`: trip list, trip detail, item list and map
view. Clients that render them get an interactive view; clients that don't get the text
response and lose nothing.

## Privacy

Your assistant calls this server; the server never sees your conversation with it. What
arrives is the tool call and its arguments, and what's stored is the trip data you'd see in
the app. API keys are stored as a SHA-256 hash and can be revoked at any time from
Settings. Full detail in the
[privacy policy](https://www.aitrips.io/privacy?utm_source=github&utm_medium=mcp-directory),
section 4.

## Links

- [Website](https://www.aitrips.io/?utm_source=github&utm_medium=mcp-directory)
- [Documentation](https://www.aitrips.io/docs?utm_source=github&utm_medium=mcp-directory)
- [Support](mailto:support@aitrips.io)
- [Official MCP Registry entry](https://registry.modelcontextprotocol.io/v0/servers?search=aitrips)

## Licence

MIT. See [LICENSE](./LICENSE).
