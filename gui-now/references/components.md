# gui.now Component Library

`gui-components.js` is injected into every canvas, free or Pro. No script tags or imports needed: just use the HTML tags below. There are 8 tags, and every one of them works without a Pro key.

JSON attributes (`data`, `rows`, `columns`, `events`, `fields`) must be valid JSON inside single quotes. Escape a `'` inside them as `&#39;`.

## Charts

```html
<!-- Bar chart (the default type) -->
<gui-chart type="bar" title="Signups" data='[
  {"label": "Jan", "value": 120},
  {"label": "Feb", "value": 180},
  {"label": "Mar", "value": 95}
]'></gui-chart>

<!-- Line chart: give it at least two points -->
<gui-chart type="line" title="Daily active users" data='[
  {"label": "Mon", "value": 42},
  {"label": "Tue", "value": 58},
  {"label": "Wed", "value": 35}
]'></gui-chart>

<!-- Pie chart -->
<gui-chart type="pie" title="Browsers" data='[
  {"label": "Chrome", "value": 65},
  {"label": "Safari", "value": 19},
  {"label": "Firefox", "value": 16}
]'></gui-chart>
```

Attributes: `type` (`bar`, `line` or `pie`), `data` (JSON array of `{"label", "value"}` with numeric values), `title`, `height` (plot height in px), `color` (bar and line color, any CSS color).

There is no radar or heatmap chart type. For anything else, draw your own SVG or canvas in the page.

## Tables

```html
<gui-table sortable title="Team" columns='["name", "role", "status"]' rows='[
  {"name": "Alice", "role": "Engineer", "status": "Active"},
  {"name": "Bob", "role": "PM", "status": "Away"},
  {"name": "Carol", "role": "Designer", "status": "Active"}
]'></gui-table>

<!-- Custom header text -->
<gui-table columns='[
  {"key": "name", "label": "Name"},
  {"key": "mrr", "label": "MRR ($)"}
]' rows='[
  {"name": "Acme", "mrr": 4200},
  {"name": "Globex", "mrr": 1800}
]'></gui-table>
```

Attributes: `columns` (JSON array of row keys, or `{"key", "label"}` objects for custom header text), `rows` (JSON array of objects keyed by those keys), `title`, `sortable` (click a header to sort), `striped`.

## Cards / Stats

```html
<gui-card title="Total Users" value="12,847" change="+12.3%"></gui-card>
<gui-card title="Revenue" value="$48.2K" change="-2.1%"></gui-card>
<gui-card title="Uptime" value="99.9%"></gui-card>
```

Attributes: `title`, `value`, `change` (optional; start it with `+` or `-` to show it rising or falling), `icon` (optional short text or emoji).

## Code Blocks

```html
<gui-code lang="python">
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
</gui-code>

<gui-code lang="html" title="index.html">
&lt;h1&gt;Hello&lt;/h1&gt;
</gui-code>
```

Attributes: `lang` (shown as the block's label), `title` (label override). The code is the tag's text. The browser parses the tag's contents first, so write `<` as `&lt;` and `&` as `&amp;` when showing HTML or XML.

## Kanban Boards

```html
<gui-kanban title="Sprint" columns='[
  {"title": "Backlog", "items": [{"title": "Research API"}, {"title": "Write docs", "tag": "docs"}]},
  {"title": "In Progress", "items": [{"title": "Build auth", "description": "Email links first"}]},
  {"title": "Done", "items": [{"title": "Set up CI"}, {"title": "Deploy staging"}]}
]'></gui-kanban>
```

Attributes: `columns` (JSON array of `{"title", "items"}`), `title`. Each item is an object with `title` and optional `description` and `tag`, not a plain string.

## Timelines

```html
<gui-timeline title="Launch plan" events='[
  {"time": "Mar 1", "title": "Kickoff", "description": "Project started"},
  {"time": "Mar 15", "title": "Alpha", "description": "Internal testing"},
  {"time": "Apr 1", "title": "Launch", "description": "Public release"}
]'></gui-timeline>
```

Attributes: `events` (JSON array of `{"title", "description", "time"}`; `description` and `time` are optional), `title`, `color` (dot color).

## Forms

```html
<gui-form title="Sign up" submit="Join" fields='[
  {"name": "name", "type": "text", "label": "Full name", "placeholder": "Jane Doe"},
  {"name": "email", "type": "email", "label": "Email"},
  {"name": "role", "type": "select", "label": "Role", "options": ["Engineer", "Designer", "PM"]},
  {"name": "notes", "type": "textarea", "label": "Notes"}
]'></gui-form>
```

Attributes: `fields` (JSON array of `{"name", "label", "type", "placeholder", "options"}`), `title`, `submit` (button text, default "Submit"), `action` (URL to send submissions to).

Field types: `select` (with `options`), `textarea`, or any HTML input type (`text`, `email`, `number`, `date`, `color`, `range` and so on). gui.now doesn't store submissions; set `action` to your own endpoint if you need them. Every field syncs across viewers as they type, like any other input on the page.

## Grid Layout

```html
<gui-grid cols="2">
  <gui-card title="Users" value="1,247"></gui-card>
  <gui-card title="Revenue" value="$8.4K"></gui-card>
  <gui-card title="Growth" value="+23%"></gui-card>
  <gui-card title="Churn" value="1.8%"></gui-card>
</gui-grid>

<gui-grid cols="2" gap="24">
  <gui-chart type="bar" data='[{"label": "Q1", "value": 42}, {"label": "Q2", "value": 58}]'></gui-chart>
  <gui-table columns='["region", "sales"]' rows='[{"region": "EU", "sales": 120}, {"region": "US", "sales": 180}]'></gui-table>
</gui-grid>
```

Attributes: `cols` (number of columns, default 2), `gap` (px, default 16). The grid keeps its column count at every screen width, and a `gui-card` is about 200px wide at minimum, so two cards side by side need about 450px and scroll sideways on a phone. For pages people will open on a phone, use `cols="2"` at most, and `cols="1"` for cards.

## Input Sync

Standard HTML form elements sync across all viewers automatically, no setup:

```html
<input type="text" name="search" placeholder="Search...">
<input type="range" name="volume" min="0" max="100" value="50">
<select name="theme">
  <option>Dark</option>
  <option>Light</option>
</select>
<textarea name="notes" placeholder="Shared notes..."></textarea>
```

Give each input a `name` or `id` so it syncs reliably. Every viewer sees the same values updating live.
