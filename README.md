# aitrips MCP Server

[aitrips.io](https://www.aitrips.io) is an AI-native trip planning platform. This MCP server lets AI assistants like Claude, ChatGPT, and Gemini create and manage trips with itinerary items, locations, tasks, notes, budget tracking, collaboration, and interactive map visualization.

## Setup

1. Sign up at [aitrips.io](https://www.aitrips.io)
2. Go to **Settings** to generate your API key
3. Add the configuration below to your MCP client

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

### Other MCP Clients

Use the Streamable HTTP endpoint `https://www.aitrips.io/api/ai/mcp` with an `Authorization: Bearer YOUR_API_KEY` header.

## Tools

### Trip Management

| Tool | Description |
|------|-------------|
| `trip_create` | Create a new trip with destination, dates, budget, and timezone |
| `trip_get` | Get full trip details including items, locations, and collaborators |
| `trip_list` | List all your trips with optional status filter |
| `trip_update` | Update trip details (name, dates, budget, status) |
| `trip_delete` | Delete a trip and all associated data |

### Itinerary Items

| Tool | Description |
|------|-------------|
| `trip_item_add` | Add activities, accommodation, flights, meals, or transport to a trip |
| `trip_item_list` | List items with filters by type, status, or date range |
| `trip_item_update` | Update item details, status, or booking info |
| `trip_item_delete` | Remove an item from a trip |
| `trip_transport_add` | Add transport with automatic origin/destination geocoding |

### Locations

| Tool | Description |
|------|-------------|
| `trip_location_add` | Save a location with coordinates and type |
| `trip_location_list` | List saved locations with optional filters |
| `trip_location_nearby` | Find saved locations within a radius |
| `trip_location_delete` | Delete a saved location |

### Tasks & Reminders

| Tool | Description |
|------|-------------|
| `trip_task_create` | Create a task with priority and due date |
| `trip_task_list` | List tasks with status filter |
| `trip_task_update` | Update task details |
| `trip_task_complete` | Mark a task as completed |
| `trip_task_delete` | Delete a task |

### Notes

| Tool | Description |
|------|-------------|
| `trip_note_create` | Create a free-form markdown note |
| `trip_note_list` | List notes with optional date filter |
| `trip_note_update` | Update note content |
| `trip_note_delete` | Delete a note |

### Sharing & Collaboration

| Tool | Description |
|------|-------------|
| `trip_share_create` | Create a shareable link with viewer or editor permissions |
| `trip_share_list` | List active share links |
| `trip_share_revoke` | Revoke a share link |
| `trip_collaborators_list` | List trip collaborators |
| `trip_collaborators_remove` | Remove a collaborator |

### Search

| Tool | Description |
|------|-------------|
| `search` | Search across all trips, items, and locations |
| `fetch` | Fetch full details for a search result |

## Links

- [Website](https://www.aitrips.io)
- [Documentation](https://www.aitrips.io/docs)
