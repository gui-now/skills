# gui.now Full API Reference

## Endpoints

### Create Canvas
```
POST https://gui.now/api/canvas
Content-Type: application/json
```

Body:
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `html` | string | One of html/markdown required | Raw HTML content. Use `<gui-*>` component tags directly. |
| `markdown` | string | One of html/markdown required | Server-rendered. Component tags work here too. |
| `title` | string | No | Display name, OG meta |
| `frames` | array | No | `[{"html": "...", "label": "Tab Name"}]` |
| `theme` | string | No | `dark` (default) or `light` (markdown only) |
| `expires` | string | No | A duration like `30m`, `24h`, `7d` (up to `365d`), or `never`. Without an account: 24h default, 72h max. Pro key: never by default. Past your max you get the max and `expires_clamped: true`; anything else is a 400. |
| `password` | string | Pro only | Password-protect the canvas |

Pro header: `x-api-key: YOUR_PRO_KEY`

Response:
```json
{
  "id": "abc123xyz",
  "url": "https://gui.now/abc123xyz",
  "edit_token": "EDIT_TOKEN",
  "expires_at": "2026-03-07T12:00:00Z",
  "pro": false,
  "format": "html"
}
```

### Update Canvas
```
PUT https://gui.now/api/canvas/:id
Authorization: Bearer EDIT_TOKEN
Content-Type: application/json

{"html": "<h1>Updated</h1>"}
```

### Extend Expiry
```
POST https://gui.now/api/canvas/:id/extend
Authorization: Bearer EDIT_TOKEN
Content-Type: application/json

{"expires": "7d"}
```
With no body it adds 24 hours from now. With the edit token, `expires` sets how long from now (same format as create, capped at your plan's max). Extending never shortens a page.

### Mermaid Diagrams
```
POST https://gui.now/api/flow
Content-Type: application/json

{"mermaid": "graph TD\n  A-->B", "title": "My Flow"}
```

### SSE Real-Time Events
```
GET https://gui.now/api/canvas/:id/events
```
Events: `connected` (on subscribe), `update` (html/title/frames changed).

## Error Codes

| Code | Meaning | User Message |
|------|---------|-------------|
| 400 | Invalid body | Check JSON format |
| 402 | Pro feature on free tier | "This feature requires Pro — gui.now/pro" |
| 413 | Content too large | "Over 2MB limit. Pro supports 4MB — gui.now/pro" |
| 429 | Rate limited | "Rate limit reached. Pro allows 100/hr — gui.now/pro" |

## SDKs

### JavaScript / TypeScript
```bash
npm install gui-now-sdk
```
```javascript
import { createGui } from 'gui-now-sdk'
const gui = createGui()                           // free
const gui = createGui({ apiKey: 'PRO_API_KEY' })  // pro

const canvas = await gui.create({ html: '<h1>Hello</h1>' })
console.log(canvas.url)

await gui.update(canvas.id, canvas.edit_token, { html: '<h1>v2</h1>' })
```

### Python
```bash
pip install gui-now
```
```python
from guinow import GuiNowClient

# Free tier. For Pro: GuiNowClient(api_key="PRO_KEY"), or set GUI_NOW_API_KEY.
client = GuiNowClient()
canvas = client.create(html="<h1>Hello</h1>", title="My Canvas")
print(canvas.url)

client.update(canvas.id, canvas.edit_token, html="<h1>Updated</h1>")
```

### cURL
```bash
# Create
curl -X POST https://gui.now/api/canvas \
  -H 'Content-Type: application/json' \
  -d '{"html": "<h1>Hello</h1>"}'

# Update
curl -X PUT https://gui.now/api/canvas/CANVAS_ID \
  -H 'Authorization: Bearer EDIT_TOKEN' \
  -H 'Content-Type: application/json' \
  -d '{"html": "<h1>Updated</h1>"}'

# Pro
curl -X POST https://gui.now/api/canvas \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: YOUR_PRO_KEY' \
  -d '{"html": "<h1>Pro Canvas</h1>", "expires": "90d"}'
```
