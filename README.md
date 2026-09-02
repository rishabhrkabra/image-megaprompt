# image-megaprompt

A Claude Code skill for writing image-generation prompts that render the same
way twice.

## The problem

Asking an image model for UI or section visuals usually goes the same way. The
first gallery is promising, the second drifts, option 4 arrives as a sketch
because the model got bored, and somewhere around round three it invents a
statistic, a tagline, or an interface nobody asked for.

Most of that is not a prompt-wording problem. Image models do not carry a
conversation's worth of rules; they render what is in front of them, right now.
A rule agreed three messages ago is not in front of them. Neither is the style
you settled on last week.

So this skill writes every prompt as one self-contained JSON envelope that
carries its own style law, its own copy, and its own delivery contract — and
can be pasted into a fresh chat with no history. Fixed keys in a fixed order,
so revisions are diffable and a prompt that worked can be reused instead of
rewritten from memory.

## The envelope

Thirteen keys. The ones doing the real work:

- **`locked_*`** — one field per invariant. Background, typography, colour,
  materials, grid. Naming each one separately is what stops a settled system
  getting quietly summarised away between revisions.
- **`section_spec`** — one entry per slot, each stating its own content,
  layout and emphasis inline. Never a pointer to shared data.
- **`output_protocol`** — how many images, at what quality, in how many
  messages, and the commands for iterating on one of them.
- **`negatives`** — the accumulated list of what this model keeps doing wrong.

`SKILL.md` has the full specification and the rules for filling it.

## Three rules that carry most of the weight

**Exact count is a delivery contract.** Ask for four images, get four final
images at identical quality, in one response. A re-render replaces its numbered
slot; it never arrives as a fifth image. Without this stated explicitly,
models routinely send one good image and three thumbnails.

**Fabricate every string of sample data.** The prompt leaves your machine.
Write plausible neutral content instead of real names, employers, figures or
personal facts — and check the invented names against the real people on the
project so no placeholder lands on someone actual.

**Never ask for assets the model does not have.** It will not decline; it will
fabricate something convincing and wrong. That covers brand marks, real
screenshots, and anything from a product that is not public. Reserve an empty
rectangle at a stated aspect ratio and composite the real asset in afterwards,
or represent the thing as a diagram or abstraction instead.

## What's inside

| File | What it is |
|---|---|
| `SKILL.md` | The envelope specification and the rules for filling it |
| `examples/analytics-dashboard.md` | A complete filled envelope — four dashboard layouts, all data fabricated. Paste-ready |
| `agents/openai.yaml` | Interface metadata for agent runners that read it |

## Install

This is a [Claude Code](https://claude.com/claude-code) skill. Drop the repo
into a skills directory:

```bash
# available in every project
git clone https://github.com/rishabhrkabra/image-megaprompt ~/.claude/skills/image-megaprompt
```

```bash
# or scoped to one project
git clone https://github.com/rishabhrkabra/image-megaprompt <project>/.claude/skills/image-megaprompt
```

Claude Code discovers it automatically from the `SKILL.md` frontmatter. Ask for
an image or mockup prompt and it loads, or invoke it directly with
`/image-megaprompt`.

## Scope

Use it for prompts aimed at an external image model — UI mockups, section
visuals, concept galleries, and production plates that a live site renders text
over.

Do not use it for editable Figma output or HTML prototypes. Those are build
tasks, and an image is the wrong artifact.

## Licence

MIT. See [LICENSE](LICENSE).
