# let AI think about doing…

A tiny, single-file web app that turns any goal into an executable plan.

Give it a goal and it uses **your own AI API key** to:

1. **Recursively decompose** the goal into a tree of ever-smaller parts, down to **basic operation units** — the smallest atomic steps (e.g. `build rocket → head, body, tail, fuel → …`).
2. **Generate a grounded, hallucination-resistant prompt** for each basic operation, so you can paste them into a fresh AI chat one at a time and actually get the thing done.

No backend, no build step, no telemetry. It's one `index.html` that runs entirely in your browser.

## Usage

1. Open [`index.html`](index.html) in any modern browser (just double-click it).
2. **Connect your AI** — choose Anthropic (Claude) or OpenAI (GPT), paste your API key, pick a model, and click *Test connection*.
3. **Enter a goal**, choose an auto-breakdown depth, and click *Break it down*.
4. **Refine the tree** — break nodes down further, mark the smallest ones as *basic operations*, edit or delete nodes.
5. **Generate execution prompts** for every basic operation and copy them one by one.

## Privacy & keys

- Your API key is stored only in your browser's `localStorage` and is sent **directly** from your browser to the AI provider — it never passes through any other server.
- For Anthropic, the app sends the `anthropic-dangerous-direct-browser-access` header so browser calls are permitted. Because the key lives in the browser, run this on your own machine, not a shared/hosted page.

## How the anti-hallucination prompts work

Each generated prompt is deliberately scoped to a single atomic operation and includes:

- a tightly-scoped expert **role**,
- the one concrete **task**,
- the **inputs needed** — with an instruction to *ask rather than invent* anything missing,
- explicit **anti-hallucination rules** (no fabricated numbers/specs/sources; flag uncertainty),
- the exact **output** form and a one-line *done* check.

## Tech

Plain HTML/CSS/JS, no dependencies. Supports the Anthropic Messages API and the OpenAI Chat Completions API.
