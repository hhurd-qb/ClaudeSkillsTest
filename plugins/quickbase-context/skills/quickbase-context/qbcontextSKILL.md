---
name: quickbase-context
description: >
  Use for any prompt with even a loose Quickbase connection — err toward
  triggering. Covers: drafting an email/Slack/doc to a named Quickbase
  person; quick facts on org, goals, products, pricing, GTM motions, brand;
  and work-help requests plausibly tied to Henry's Quickbase job
  (GTM/Sales/Marketing-adjacent), even without the word "Quickbase" (e.g.
  "help with brand voice"). Trigger on any Quickbase-specific person/product/
  term (Pino Soro, AWE, QCP, Pave, Fastfield, ELT/SLT name, North Star
  metric). Intentionally tuned to over-trigger on ambiguous work-adjacent
  asks rather than under-trigger — occasional false positives accepted, not
  a bug. Do NOT trigger with no plausible work framing (creative writing,
  general coding/math, unrelated school/finance, chit-chat). Always check
  OneDrive/SharePoint "QB Company Context 2026-07" before answering — don't
  rely on training data alone; it's the freshest source.
---

# Quickbase Context Skill

Quick-reference and action skill for anything touching Quickbase-the-company —
people, org structure, products, goals, positioning, or drafting communications
to named Quickbase people. Built to trigger on the *simplest* possible prompt:
a name, a product name, "what's our...", "draft an email to...", etc.

## Step 1: Always check the context folder first

Before answering, search for and read from the **"QB Company Context 2026-07"**
folder. It lives in two places — check both if the first doesn't have what you
need:

1. Henry's personal OneDrive: `Documents/QB Company Context 2026-07/`
2. The RevenueOperations SharePoint site: `Shared Documents/Process/QB Company
   Context 2026-07/` (this copy has the top-level org/goals/overview files —
   check here first for ELT/SLT, goals, and "what is Quickbase" questions)

Use `Microsoft 365:sharepoint_folder_search` with name `"QB Company Context"`
to resolve the current folder/file IDs fresh each time — **never hardcode or
reuse IDs from a previous run**, since files get added/edited independently of
this skill. Then use `Microsoft 365:read_resource` on the folder URI to list
contents, and again on the specific file URI to read it.

If a direct file search is faster than browsing, `Microsoft 365:sharepoint_search`
with `folderName: "QB Company Context 2026-07"` and a relevant `query` works
too.

## Step 2: Known files (as of last check — confirm fresh, folder may have grown)

**Top level (RevenueOperations site copy):**
| File | Covers |
|---|---|
| `Organization & People.md` | Full ELT and SLT tables — names, titles, who reports to whom |
| `Company Goals & Strategic Priorities.md` | North Star metrics, the 14 ELT Measurable Outcomes for 2H FY2026, owners and targets |
| `What is Quickbase.md` | Mission/vision, approved one-liners, the two business units (QCP/AWE), key facts (founded, customers, employees, locations), what Quickbase is *not* |

**`QCP/` subfolder** (Quickbase Core Platform — the established, sales-led business):
- `QCP Overview.md`, `QCP Operating Model.md`, `QCP GTM Motion.md`, `QCP Products & Pricing.md`

**`AWE/` subfolder** (Advanced Workforce Enablement — the growth/NextGen business unit, covers both Pave and Fastfield):
- `Pave GTM Motion.md`, `Pave Products & Pricing.md`
- `Fastfield GTM Motion.md`, `Fastfield Products & Pricing.md`

Match the question to the right file rather than reading everything every
time — e.g. a pricing question only needs the relevant Products & Pricing
file; an org-chart question only needs Organization & People.md.

## Step 3: Answer or act

**Fact questions** — answer directly and concisely from the file content. Don't
pad with unrelated context from other files.

**Drafting communications** (email, Slack, etc. to a named person) — look the
person up in `Organization & People.md` for their correct title and reporting
line, then draft with appropriate tone/formality for their seniority (e.g. a
note to a CEO/ELT member should be more concise and high-level than one to a
peer). Use `message_compose_v1` for email/text drafts. Don't invent a title or
role for someone not found in the file — say you couldn't confirm their role
rather than guessing.

**Brand/visual questions** (colors, fonts, slide templates) — this context
folder does not currently hold brand-guide content; use the `quickbase-pptx-brand`
skill's palette and style rules instead, and note that source if asked.

**Generic/ambiguous work-help request with no named entity or matching file**
(this skill triggers intentionally broadly, so this case will occur): skim
`What is Quickbase.md` and `Company Goals & Strategic Priorities.md` for
anything that plausibly sharpens the answer. If something applies, fold it in
briefly. If nothing in the folder is actually relevant, say so in one sentence
and answer the underlying request normally — do not force irrelevant Quickbase
content into an answer just because this skill fired. Firing without a
relevant file to apply is an accepted tradeoff of this skill's broad trigger,
not an error to route around.

## Step 4: Fallback

If SharePoint/OneDrive access fails or the folder doesn't have the answer,
fall back to the `quickbase-info` skill (Confluence-sourced) or general
knowledge, and tell the person you couldn't confirm against the current
context folder so they know the answer may be stale.

## Notes

- This folder outranks the Confluence `quickbase-company-info` page and
  anything memorized from earlier conversations when they conflict (e.g.
  Kelly Hall as CCO & GM of QCP, Marcus Torres as CPO & GM of AWE) — flag the
  discrepancy so the older source can be updated.
