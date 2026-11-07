# gui.now Input Formats

## HTML

Most flexible. Full control over layout, styles, interactivity.

```json
{
  "html": "<!DOCTYPE html><html><head><style>body{background:#09090b;color:#fafafa;font-family:system-ui;padding:40px}</style></head><body><h1>Dashboard</h1><p>Real-time metrics</p></body></html>",
  "title": "My Dashboard"
}
```

Tips:
- Use complete HTML documents with `<html><head><style>` for complex layouts
- Inline all CSS — no external stylesheets
- JavaScript works — build interactive tools, not just static pages
- gui.now components (`<gui-chart>`, etc.) are auto-injected, just use the tags

## Markdown

Server-side rendered with syntax highlighting and dark theme. Good for documents, READMEs, reports.

```json
{
  "markdown": "# Project Report\n\n## Summary\n\nWe shipped **3 features** this sprint.\n\n```python\ndef hello():\n    print('world')\n```\n\n| Feature | Status |\n|---------|--------|\n| Auth | ✅ Done |\n| API | ✅ Done |\n| UI | 🔄 In Progress |",
  "title": "Sprint Report"
}
```

Supports: headings, bold/italic, code blocks with syntax highlighting, tables, lists, links, images, blockquotes.

Optional `"theme": "light"` (default is `"dark"`).

## Mermaid Diagrams

Rendered as pannable, zoomable SVG. Use the `/api/flow` endpoint:

```
POST https://gui.now/api/flow
Content-Type: application/json

{
  "mermaid": "graph TD\n  A[User Request] --> B{Auth?}\n  B -->|Yes| C[Process]\n  B -->|No| D[Login]\n  C --> E[Response]",
  "title": "Auth Flow"
}
```

Supports all Mermaid diagram types: flowcharts, sequence, class, state, ER, gantt, pie, mindmap, timeline.

Optional `"theme": "dark"` (default).

## Multi-Frame Canvases

Create tabbed canvases with multiple HTML views:

```json
{
  "html": "<h1>Default view</h1>",
  "frames": [
    {"html": "<h1 style='color:#ef4444'>Option A</h1>", "label": "Red"},
    {"html": "<h1 style='color:#3b82f6'>Option B</h1>", "label": "Blue"},
    {"html": "<h1 style='color:#22c55e'>Option C</h1>", "label": "Green"}
  ],
  "title": "Color Options"
}
```

The `html` field is the default view. Each frame appears as a tab in the viewer. Great for A/B comparisons, design variants, multi-step wizards.

## Using Components

`<gui-*>` component tags work directly in both HTML and Markdown. No separate format needed:

```json
{
  "html": "<gui-grid cols='2'><gui-card title='Revenue' value='$12,450' change='+8.2%'></gui-card><gui-chart type='bar' data='[{\"label\":\"Q1\",\"value\":42}]'></gui-chart></gui-grid>",
  "title": "Dashboard"
}
```

See [components.md](components.md) for all available tags and attributes.
