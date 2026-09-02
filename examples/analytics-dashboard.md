# Worked example — analytics dashboard, 4 layout options

A complete envelope, filled. Every string the image renders is written out
here; all data is fabricated. Paste the JSON block below into a **fresh** chat
with an image model and it should return four full-quality options in one
response.

Read this alongside `SKILL.md`. The things worth noticing:

- `sample_data` holds only microcopy repeated across slots. Every string unique
  to one slot lives inside that slot's own `section_spec` entry (rule 11).
- Occupancy and placement are numeric, never "prominent" or "subordinate"
  (rule 12).
- `locked_*` fields name what is frozen, so `task` can say precisely what is
  still allowed to vary.
- The negatives list is longer than the brief. That is normal and correct — it
  is the accumulated record of what this particular model keeps doing wrong.

---

```json
{
  "role": "Crisp legible product-UI rendering with editorial polish: true pixel-grid alignment, real typographic hierarchy, generous whitespace, flat vector interface with no photographic texture, no perspective and no device mockup.",

  "task": "Render 4 layout options for the OVERVIEW screen of a logistics analytics dashboard. The visual system below is LOCKED and identical across all four options. Only two things vary: the LAYOUT SKELETON and which metric is given primary emphasis. Content is fixed — every visible string verbatim from sample_data and the slot entries.",

  "output_protocol": "Gallery of EXACTLY 4 options in ONE response, one 16:10 image (~1728x1080) per option, numbered 1-4, identical full quality — no thumbnails, no sketches, no 'here is a rough version' for later options. Caption each: 'Option N — [layout skeleton] — [primary metric]'. Iteration: 'again N + notes' re-renders that slot solo at full fidelity; 'style B N' applies style_freeze_swap_B to that slot.",

  "self_contained": "This prompt carries its full specification in words. No reference images are supplied or needed. Do not ask for references and do not assume a house style beyond what is written here.",

  "locked_grid": "LOCKED — 12-column grid, 24px gutters, 32px page margins, 8px base spacing unit. Every element edge lands on the 8px grid. Card corner radius is 8px everywhere, never mixed.",

  "locked_typography": "LOCKED — one neutral grotesk family (Inter, Helvetica Now or equivalent) at four sizes only: METRIC 40px semibold, HEADING 18px semibold, BODY 14px regular, LABEL 12px medium uppercase with +6% letter-spacing. Nothing renders at a fifth size. Numerals are tabular everywhere so columns align.",

  "locked_colour": "LOCKED — light interface. Page #F7F8FA, cards #FFFFFF, primary text #101828, secondary text #667085, hairlines #EAECF0. Exactly ONE accent, #2563EB, and its total budget is under 5% of pixels — it appears on the active nav item, one primary button, and the single emphasised data series. Status colours are permitted only inside status pills. No gradients anywhere.",

  "locked_charts": "LOCKED — charts are clean and flat: no 3D, no drop shadows, no bevels, no donut charts, no pie charts, no gauges. Axis labels at LABEL size in secondary text. Gridlines are horizontal only, in the hairline colour. A line series is 2px with no area fill unless a slot names one.",

  "scope_lock": [
    "Static UI mockup only — no browser chrome, no device frame, no cursor, no hands, no drop shadow around the screen",
    "Execute the given information architecture — never reorder or reinvent the sections",
    "Every visible string verbatim from this prompt and correctly spelled in real English",
    "All locked systems apply identically to all four options"
  ],

  "style_freeze": "Flat, calm, information-dense enterprise UI in the register of Linear, Stripe Dashboard and Vercel Analytics: high whitespace, strong left alignment, quiet borders, no decorative illustration, no rounded-blob shapes, no marketing gloss. Cards are defined by a 1px hairline and generous padding rather than by shadow.",

  "style_freeze_swap_B": "Same locked systems, same layout and content, inverted to a dark interface: page #0C111D, cards #161B26, primary text #F5F6F7, secondary text #94A3B8, hairlines #1F2733. Accent stays #2563EB. Contrast ratios must remain legible.",

  "section_spec": [
    "SLOT 1 — SKELETON: left sidebar 240px, four KPI cards in one row across the top of the main area, a wide line chart beneath them at full content width, and a table below the fold edge. PRIMARY METRIC: On-time delivery rate, emphasised by being the only KPI card whose value is rendered in the accent colour. Chart occupies max 34% of canvas height.",
    "SLOT 2 — SKELETON: top horizontal nav instead of a sidebar, then a two-column split — the left column 8 units wide holding the line chart, the right column 4 units holding the four KPIs stacked vertically. PRIMARY METRIC: Average transit time, given the largest card in the stack. No table in this option.",
    "SLOT 3 — SKELETON: left sidebar 240px, one full-width hero metric band across the top with a single large number and a small sparkline, then a 2x2 grid of equal cards beneath it. PRIMARY METRIC: Shipments in transit, rendered at 72px in the hero band — this is the ONE slot where a fifth type size is permitted, and only for that number.",
    "SLOT 4 — SKELETON: left sidebar 240px, no KPI cards at all — the four metrics run as a single compact inline strip directly under the page title, separated by hairline dividers, with the table promoted to be the main object of the screen at full content width. PRIMARY METRIC: Exceptions raised, marked with a status pill. Table shows exactly 6 rows."
  ],

  "sample_data": {
    "product_name": "Freightline",
    "page_title": "Overview",
    "nav_items": ["Overview", "Shipments", "Carriers", "Exceptions", "Reports", "Settings"],
    "active_nav": "Overview",
    "date_range_control": "Last 30 days",
    "primary_button": "Export report",
    "kpis": [
      {"label": "ON-TIME DELIVERY RATE", "value": "94.2%", "delta": "+1.8% vs prior period"},
      {"label": "AVERAGE TRANSIT TIME", "value": "3.4 days", "delta": "−0.2 days vs prior period"},
      {"label": "SHIPMENTS IN TRANSIT", "value": "1,284", "delta": "+112 vs prior period"},
      {"label": "EXCEPTIONS RAISED", "value": "37", "delta": "−9 vs prior period"}
    ],
    "chart_title": "On-time delivery rate",
    "chart_axis_labels": ["Apr 01", "Apr 08", "Apr 15", "Apr 22", "Apr 29"],
    "chart_y_labels": ["80%", "85%", "90%", "95%", "100%"],
    "table_title": "Recent exceptions",
    "table_columns": ["SHIPMENT", "CARRIER", "LANE", "RAISED", "STATUS"],
    "table_rows": [
      ["SHP-40192", "Northwind Freight", "Pune → Nagpur", "2 hours ago", "Open"],
      ["SHP-40188", "Cardinal Logistics", "Surat → Indore", "5 hours ago", "Investigating"],
      ["SHP-40175", "Meridian Transport", "Kochi → Bengaluru", "Yesterday", "Resolved"],
      ["SHP-40171", "Northwind Freight", "Jaipur → Delhi", "Yesterday", "Open"],
      ["SHP-40166", "Blue Harbour Lines", "Chennai → Hyderabad", "2 days ago", "Resolved"],
      ["SHP-40160", "Cardinal Logistics", "Ludhiana → Kanpur", "2 days ago", "Resolved"]
    ],
    "status_pill_values": ["Open", "Investigating", "Resolved"],
    "empty_state_never_shown": "Not applicable — every option renders populated data"
  },

  "negatives": [
    "NO browser chrome, URL bar, traffic-light dots, device frame, tablet or phone mockup, cursor or hands",
    "NO 3D charts, donut charts, pie charts, gauges, radial progress or bevelled bars",
    "NO gradients, glassmorphism, blurred translucent panels, glow, or neon",
    "NO decorative illustration, mascots, isometric scenes, abstract blobs or background patterns",
    "NO emoji anywhere, including inside status pills",
    "NO lorem ipsum, placeholder text, or invented strings — every string comes from this prompt",
    "NO misspellings; all copy is real, correctly spelled English",
    "NO watermark, logo, signature, handle or attribution of any kind",
    "NO more than four type sizes, except the single hero number in slot 3",
    "NO second accent colour; the accent budget stays under 5% of pixels",
    "NO drop shadows on cards — hairline borders only",
    "NO timeline, roadmap or calendar as an organising structure",
    "NO real company names beyond the fabricated ones listed in sample_data",
    "NO dark mode unless 'style B N' is invoked"
  ],

  "kickoff": "Render all 4 options now in one response, numbered, at identical full quality. No questions, no pauses, no preamble. Fresh chat — nothing before this message applies."
}
```

---

## What to do with the result

Triage the gallery first: pick the skeleton that works, ignore small defects.
Then `again 2 + the KPI stack is too tight, double the vertical gap between
cards` to get one option at solo quality. Each rejected round adds a line to
`negatives` before the next prompt goes out.
