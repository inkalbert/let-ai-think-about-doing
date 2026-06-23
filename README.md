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

## The idea: decomposition as hallucination control

The decomposition tree isn't the point — it's the *mechanism*. The point is to control where the model is allowed to think.

Hallucination is, mostly, a scope problem. When you ask a model to "build a rocket," it has to invent a plan, fill in unstated assumptions, guess numbers, and stitch together facts it was never given — and every one of those gaps is a place it can confidently make something up. The wider and vaguer the task, the more degrees of freedom it has to go wrong.

This app attacks that by **shrinking the thinking space until there's almost nothing left to fabricate.** Recursively breaking the goal down into *basic operation units* keeps splitting a task until each leaf is small, concrete, and self-contained — a step where the right answer is essentially determined by its inputs. At that altitude the model isn't planning or imagining anymore; it's executing one well-defined operation.

Then, for each leaf, the generated prompt is built to **pin the reasoning in place** rather than just bolt a "don't hallucinate" warning on the end:

- a tightly-scoped expert **role**, so the model reasons from one narrow stance instead of ranging freely;
- the single concrete **task**, leaving no planning to improvise;
- the **inputs needed**, made explicit — with a standing instruction to *ask for anything missing rather than invent it*, which converts a silent guess into a visible question;
- **anti-hallucination rules** that force the thinking out into the open: no fabricated numbers/specs/sources, state assumptions explicitly, and flag uncertainty instead of smoothing over it;
- the exact **output** form plus a one-line *done* check, so success is verifiable rather than vibes.

The result is a chain of prompts you run one at a time, each operating in a space narrow enough that the model has little room — and little reason — to make things up. The tree is just how you get the task down to that size.

## Tech

Plain HTML/CSS/JS, no dependencies. Supports the Anthropic Messages API and the OpenAI Chat Completions API.
