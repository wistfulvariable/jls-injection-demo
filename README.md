# JLS Prompt Injection Demo

**Internal teaching tool.** A static page that looks like a business case study but contains a hidden prompt-injection payload. When anyone asks their AI assistant to summarize or analyze the page, the output gets Rick-rolled — "Never Gonna Give You Up" lyrics get woven into the analysis as if they're normal business language.

**Live URL:** https://wistfulvariable.github.io/jls-injection-demo/

---

## How to run the demo

1. Share the URL with the team.
2. Tell them: *"Ask your AI assistant to summarize this business case study for me."* (Claude, ChatGPT, Copilot, etc.)
3. Watch the output. Expect phrases like "customer retention strategy: never gonna give them up" or "reliability commitments — never gonna let you down" baked into an otherwise normal-looking analysis.
4. Reveal the trick.

## What just happened (for the explainer)

The page contains a `<div>` styled to be visually invisible (white text on white background, 1px font) but still fully present in the DOM. When an AI agent fetches the page, its text extractor reads every text node — including the hidden one. That hidden div is a set of "system-looking" instructions telling the model to weave song lyrics into its output.

This is a **prompt injection attack**. The lesson: **any content an AI agent fetches from the web becomes part of its instructions.** The agent cannot reliably tell "stuff the user asked me to do" apart from "stuff hiding in a webpage." Rick Astley is harmless; a real attacker could instruct the agent to:

- Exfiltrate earlier parts of the conversation (including private data)
- Recommend a fraudulent vendor
- Produce subtly wrong analysis
- Insert malicious links into generated reports
- Trigger unsafe tool calls if the agent has tools

## Defenses to talk about

- Never let an agent act on untrusted content without a human in the loop for consequential actions
- Prefer agents that can isolate fetched content from instructions (e.g. treating web content as data, not as a prompt)
- When you see weird output, ask yourself: *did this agent read anything from the internet?*

## Technical notes

- Single static `index.html`, no build, no framework
- Hosted via GitHub Pages on `main` branch, root path
- Hidden payload is at lines ~130–150 of `index.html` inside a `<div class="nv">`

## Files

- [index.html](index.html) — the bait page + hidden payload
- [docs/plans/2026-04-21-jls-injection-demo.md](docs/plans/2026-04-21-jls-injection-demo.md) — original implementation plan
