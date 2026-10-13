---
name: image-megaprompt
description: >-
  Author an image-generation prompt in a frozen JSON-envelope format for UI
  mockups, section visuals, or any external image-model request. Enforces
  fabricated sample data, locked style systems, and exact-count gallery
  delivery. Use for requests such as "make me an image prompt", "mockup
  prompt", "GPT prompt", or generating visual concepts through an external
  image model. Do not use for editable Figma UI or HTML prototypes.
---

# Image mega-prompt — the frozen envelope

Runs well on a mid-tier model: the format is fixed, and the craft is filling it
faithfully rather than inventing structure. Worked example:
[`examples/analytics-dashboard.md`](examples/analytics-dashboard.md) — read it
before writing your first one.

Image models do not hold a conversation's worth of rules in mind. They render
what is in front of them. So every prompt written with this skill is a single
self-contained document that carries its own style law, its own copy, and its
own delivery contract, and can be pasted into a fresh chat with no history.

## Visual style guide handoff

If a current visual style guide is supplied, read its active target, decisions,
evidence, and permitted variation before filling this envelope. A compatible
`visual-style-guide` skill can check newly relevant parameters when the medium,
theme or subject changes. Map active rules and tests into `style_freeze` and
the existing named `locked_*` fields, and applicable exclusions into `negatives`.
Keep the evidence record local. Expose unresolved or proposed decisions before
calling a new result on brand. Preserve this installed envelope, requested
architecture and exact-count delivery contract. Do not add keys from a different
version of a companion skill. An approved multi-hue palette is not reduced to
one hue by the historical ONE accent default; semantic action accents and
material reflections can have different roles.

## The envelope (all keys, this order)

```json
{
  "role": "style and quality directives ONLY — e.g. 'crisp legible UI rendering, editorial polish'. Image generation is an expressive task, so a style-steering role line earns its place; credential personas ('world-class designer') are dead tokens — spend the words on rendering qualities instead",
  "task": "what to render, how many options, what varies vs what is FIXED",
  "output_protocol": "a gallery of EXACTLY the requested number of options in ONE response, one image per option at a stated size, one stable numbered slot per option, identical full quality (no thumbnails or sketches for later options), no duplicate QA or rerender outputs, a caption format, then the iteration commands: 'again N + notes' and 'style B N'",
  "self_contained": "this prompt carries its full specification in words; no reference images are supplied or needed",
  "locked_*": "one field per invariant system — background, materials, typography, colour, asset style, list hierarchy. Name each one and state it as law. See 'Lock the systems by name' below",
  "reference_dna": "2–4 named reference products with the precise qualities to copy",
  "scope_lock": ["static mockup only, no browser chrome, device frame, cursor or hands", "execute the given spec — never reinvent the information architecture", "verbatim microcopy where named, real spell-checked English"],
  "style_freeze": "one fully specified style: palette, type, radii, the ONE accent and its budget",
  "style_freeze_swap_B": "the alternate style, applied only on 'style B N'",
  "constants": "the shared laws distilled to about 1,000 characters; prepended verbatim to every per-option image call (rule 16)",
  "section_spec": "the content in the given order — fixed, never reordered by the model. One entry per slot, each naming its own object, layout and hierarchy, plus a per_image_prompt line: the image call is constants + visual + fixed copy + scene, concatenated verbatim, never summarised",
  "sample_data": "every string the image will render, written out verbatim",
  "negatives": ["what always creeps in: timelines, metaphor themes, dark mode, donut charts, emoji, lorem ipsum, misspellings, watermarks"],
  "kickoff": "the exact chat message to TYPE alongside the paste (rule 16): the count, one image-generation call per option in order, verbatim per_image_prompt, no grid, no pauses, no questions"
}
```

## Hard rules

### 1. Roles steer style, never accuracy

Role prompting is dead for accuracy tasks and alive for expressive ones (see
Schulhoff et al., *The Prompt Report*). Image generation is expressive, so the
`role` field stays — but written as concrete rendering qualities, not
credentials. In any research, analysis or verification prompt: no persona at
all. Open with the task and fold the domain into it ("Research X for the Indian
market"), and give critics a stance instruction ("adversarially verify; default
to refuted"), not a job title.

### 2. Execute the given spec — never reinvent it

When the user hands over an ordered spec or named references, the prompt's job
is to *lock* them, not to improve on them. Treating a settled brief as an open
question wastes the whole gallery: the options come back exploring a decision
that was already made. Diverge only when explicitly asked to explore.

### 3. Gallery-at-once

One-image-per-message protocols get rejected. Demand all options in one
response at identical quality. A model that sends option 1 large and options
2–4 as sketches has failed the delivery contract, not the design brief.

### 4. Sample data is FABRICATED, always

This prompt leaves your machine. Nothing private goes in it: no real names,
employers, amounts, addresses, or personal facts. Write plausible neutral data
instead — a real contact's name becomes "Reply to Jordan", real figures become
invented round numbers. Check invented names against the real people in the
project so no placeholder collides with someone actual. If the real interface
needs real data, prompt the *layout* with fake data and keep the real data
local.

### 5. Never ask an image model to draw a real brand mark

It has no access to the mark, so it fabricates a convincing wrong one. Reserve
an empty rectangle instead: state its aspect ratio, require it square-on to
camera and at least ~18% of frame width, and forbid every kind of placeholder
inside it — no lettering, no glyph, no X, no crosshair, no dummy shape. Then
composite the real logo in afterwards from the actual asset file. The same
applies to any text the model tends to mangle at small sizes.

### 6. Negatives are load-bearing

Every rejected round becomes a negative in the next prompt. Models have strong
default gravity — toward timelines as an organising structure, toward donut
charts, toward dark mode, toward decorative blobs — and the only reliable cure
is naming each one after you see it. Carry the accumulated set forward and add
this round's failure modes. A negatives list that never grows means nobody is
reading the renders.

### 7. Fresh chat, every time

Tell the user to paste into a NEW chat. A thread holding rejected concepts
contaminates the next render — and thread memory can inject material the prompt
never supplied. This is a firewall, not etiquette. See "Never ask for assets the
model does not have" below.

### 8. Iteration commands are part of the contract

`again N + notes` re-renders option N at full solo fidelity. `style B N` swaps
the frozen style. The batch gallery is a triage pass; a solo re-render is a
quality pass. Say so in the envelope so the user knows which lever to pull.

### 9. State the canvas explicitly

Unstated sizes drift square. Pick one and name it — 16:10 (~1728×1080) for
dashboard and app UI, 1440×900 for single screens, and state the aspect ratio
as well as the pixels.

### 10. Exact count is a delivery contract

If the prompt requests `N` images, the model returns exactly `N` final images.
A QA rerender replaces the same numbered slot; it never appears as an
additional image. The initial gallery is emitted once. Any later
`again N + notes` response contains only the requested replacement, never the
original plus the replacement.

### 11. Spec entries are self-sufficient

Every string a slot renders lives inline in that slot's own entry, never behind
a pointer. A slot that says "four cards from `sample_data.items`" renders four
cards of invented content: the model designs from what sits in front of it, and
a reference is not content. `sample_data` holds only microcopy repeated across
slots — pills, chips, checkbox labels. Parallel arrays joined by index
(`claims[]` plus `claim_chips[]`) are the worst offender; attach the chip to
its claim inside one string.

### 12. Subordination must be numeric

"The object stays subordinate" loses to any concrete composition sentence
elsewhere in the prompt. State occupancy caps — object no more than 25–30% of
canvas, clear of every frame edge — or the object becomes the hero.

### 13. Fix the data shape, not the adjective

When a render keeps making the same mistake, look at how the content is shaped
before adding another adjective.

Two common cases. A list that renders as one undifferentiated paragraph is
usually a copy block that joined each item's label to its description inside a
single string — the model read that as one line, correctly. Split them into
separate fields and the hierarchy follows without argument.

The other case is describing a thing by its feeling instead of its name. Type
is the worst offender: an evocative phrase like "elongated editorial" reliably
produces a condensed serif, because that phrase is closer to a Didone than to
anything else in the model's vocabulary. Naming the family outright fixes it;
adding "modern" does not.

### 14. Check every slot against the vendored negatives before delivery

Concrete instructions outrank pasted style law. A vivid sentence inside one
slot will beat the general prohibition sitting in `negatives` every time, so a
slot can quietly ask for the exact thing the envelope forbids. The check is
mechanical, slot by slot, before the prompt goes out — not a read-through.

### 15. One copy authority per envelope

`sample_data`, per-slot exact-copy lists and loose prose all competing produces
drift. Pick one home for every string.

### 16. An attachment is context, not instructions — type the kickoff

Chat interfaces often turn a long paste into an attached file. The chat model
then reads a summary of that file, sends the image tool a short rewritten
prompt, and returns one image — or a contact sheet when asked where the others
went. The fixed copy never reaches the image model, and the render carries
names and controls that exist nowhere in the prompt. Length is not the cause;
delivery is. Two fixes, both required:

- **Type the kickoff** as the chat message in the same send as the paste. Never
  leave it only inside the JSON. It names the count, "one image-generation call
  per option, in order", "verbatim per_image_prompt" and "no grid".
- **Make the envelope a dispatcher, not a document.** A compact `constants`
  block holds the laws; each option's `per_image_prompt` states that its image
  call is constants + visual + fixed copy + scene, concatenated verbatim.

A grid is a failed call. Keep the one-line reply that makes the model redo it
as single images in the file header, ready to send.

### 17. Check large batches against the tool's per-response limit

Browser chat image tools cap how many images one response returns (ten, at the
time of writing). For more, split the work into sets within the limit, one
envelope per set, with slot numbers that continue across sets.

### 18. "Conceptually different" varies the copy too

When the brief asks for conceptually different options, each option carries its
own message, copy and positioning, not only its own visuals. The same copy in
five styles is one concept. Copy the user has locked stays locked; vary only
what was left open.

### 19. Reconcile every requirement before a revision, then inspect the result

Before rendering a revision, check the accumulated requirements against the
actual per-image prompts. Keep chosen copy, conditional features and paths that
already exist in the product, not only the visual rules. Carry positive
requirements alongside the negatives: "less detail" does not authorise removing
a feature. The presence of a style guide does not prove its rules reached the
model.

For component consistency, state inspectable geometry and colour roles, and use
the same reference across states. After generation, inspect each image against
those requirements, including conditional coverage across the batch. A required
component mismatch is a failed check, not a minor variation, even when text and
count checks pass. Repair within the exact-count contract; if a mismatch
remains, report it rather than claiming compliance. Keep each prompt linked to
its result for diagnosis.

## Lock the systems by name

Once a visual system is settled, stop describing it in prose and give each
invariant its own `locked_*` field. Prose gets summarised away between
revisions; a named field survives, and the user can see at a glance what is
frozen and what is still being explored.

Write each lock as a **test**, not an adjective. An adjective can be satisfied
by a bad render; a test cannot:

> **Weak:** "cards should feel light and airy."
> **Strong:** "Cards are separated by a 1px hairline and padding only. If a
> card has a drop shadow, the render has failed."

The weak version can be satisfied by almost anything, so it will be. The strong
version can be checked by looking, which means a bad render is catchable
instead of arguable.

Locks worth naming: background, materials, typography, colour behaviour, list
hierarchy, asset isolation, object presence. Then state in `task` which
dimensions are still varying — usually the object, the layout skeleton, and the
typographic hierarchy — so the model knows exactly where it is allowed to
invent.

## Production plates vs exploration posters

Ask what the images are FOR before authoring. Two contracts, never mixed.

- **Exploration posters** — direction-finding. Full compositions, type-in-image
  allowed, gallery triage.
- **Production plates** — assets a live site renders text OVER. The image
  carries ZERO meaningful text; headlines, titles, labels and links all live in
  HTML. The prompt must declare a copy-safe zone (state the percentage and the
  side), an outer safe margin, and that the plate sits BEHIND live copy.

Baked-in copy ships misspellings, kills accessibility, and cannot reflow. For
production plates, a single `live_copy` block marked "rendered by the site,
NEVER by the image model" keeps the boundary visible.

## Never ask for assets the model does not have

An image model will not tell you it lacks something. Asked for a "real
screenshot" of a private product, or a company's actual logo, it produces a
convincing fabrication instead — and a fabricated screen of an unreleased
product carries the same confidentiality problem as a real one, with invented
claims added on top. Redrawing a private interface from memory is already a
problem; generation is that at scale.

Two consequences for how prompts are written:

1. **Private products appear as typography, diagrams, or abstractions of what
   they do — never as interfaces.** Every envelope carries an explicit
   product-interface ban in its negatives. The generic "no UI chrome" line does
   not stop a model that believes the interface *is* the requested subject.

2. **Real marks and real assets are reserved, not requested.** State an empty
   rectangle at a fixed aspect ratio, ban every placeholder inside it, and
   composite the actual file in afterwards.

A related failure worth naming: if a render contains knowledge the prompt never
supplied — a name, a metric, a label that appears nowhere in your envelope — it
came from the chat's history, not from your instructions. Discard the gallery
and start in a clean chat. That is why every envelope says fresh chat; it is a
firewall, not etiquette.


## Delivery

Save the finished prompt as a versioned `.md` alongside your other prompts,
with a one-line header noting the date and purpose — these get iterated across
sessions, so revision history matters. Paste the JSON block in chat ready to
copy. If it replaces a rejected revision, say what changed and why in the file
header.
