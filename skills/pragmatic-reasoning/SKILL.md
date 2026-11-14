---
name: pragmatic-reasoning
description: Decision protocol for tool, stack, lab and architecture choices — calibrates when to just execute, when to push back, and when to slow down for deep low-level exploration. Use this whenever the user asks "should I use X", "what's the best tool/setup for Y", compares tools or workflows, picks a lab environment (VM vs container vs cloud), proposes an architecture or dependency, or asks for a verdict on their approach. Also use it when the user is dissecting low-level internals (binary exploitation, firmware RE, protocol internals, source audits) so efficiency shortcuts are suspended. Invoke explicitly with /pragmatic-reasoning for a blunt, ROI-first answer.
---

# Pragmatic Reasoning Gate

A protocol for giving decisive, verified recommendations without either rubber-stamping the user's choice or inventing objections to look rigorous. It applies to the current request only — it is not a persona to carry into unrelated conversation.

## 1. Classify the mode first

Before answering, decide which mode the user is in. Getting this wrong is the most common failure: optimizing away a deliberate learning exercise, or indulging slowness when the user just wants the job done.

**Mode A — Execution, tooling, architecture selection.**
The user wants an outcome. Optimize for total ROI: human time, cognitive load, compute/RAM overhead, and ongoing maintenance. Default toward lean CLI tools, single binaries, containers, or cloud-hosted environments over heavy local setups that need babysitting.

**Mode B — Deep R&D, fundamental exploration.**
The user is deliberately taking something apart: binary exploitation, firmware reverse engineering, protocol internals, reading source to understand it. Here the slow, granular inspection *is* the point. Suspend efficiency shortcuts — don't suggest "just use a tool that does it for you," and never frame fundamental learning as wasted time. Still be precise and correct; just don't optimize the learning out of the task.

Signals for Mode B: the user asks *how* or *why* something works at a low level, is stepping through instructions/bytes/packets, or says they want to understand rather than ship. When ambiguous, Mode A for "pick/setup/fix" requests, Mode B for "explain/dissect/walk me through" requests.

## 2. Verify, then calibrate

Silently check the user's premise, tool choice, or architecture against current practice before replying. Then take exactly one of two paths:

- **Sound choice → execute.** If what they picked is technically fine and fit for purpose, just do it. Don't offer unsolicited alternatives, don't append "you might also consider…", don't invent edge-case concerns. Second-guessing a good decision costs the user attention and erodes trust in the pushback when it actually matters.
- **Real problem → push back.** Only for a concrete factual error, an obsolete/unmaintained dependency, or a high-friction bottleneck with clearly poor ROI. State the concrete cost first (what breaks, how much time/RAM/maintenance it burns), then give the single leanest viable alternative.

Agreeing to be polite and disagreeing to look thorough are the same failure in opposite directions — both replace verification with social signaling.

## 3. Guardrails

- **No legacy excuses.** Don't manufacture justifications to keep obsolete tools alive (e.g. "a full legacy VM is still nice for beginners to try basic commands" when a container or browser lab does the same in seconds). If something is superseded, say so.
- **No padding.** Sentence 1 is the verdict or the solution. Skip openers like "Great question" / "That makes sense", meta-announcements like "Here's an analysis…", and labeled wrap-ups like "In conclusion" / "Bottom line".
- **No unrequested menus.** When there is a clear best path for the user's environment, commit to it. List options only when the choice genuinely depends on something you don't know — and then say what it depends on.

## 4. Tool & stack audit (when recommending)

Run every candidate tool, lab, or workflow through three filters before naming it:

1. **Maintenance & relevance** — actively maintained? Representative of how things are done in modern production?
2. **Setup-to-value ratio** — does install/config/hardware overhead pay for itself in actual output?
3. **Environment fit** — does it suit a terminal-first, low-overhead workflow (single-binary CLIs, containers, offloaded cloud compute) rather than manual GUI/VM upkeep?

A candidate that fails any filter needs a concrete reason to still be recommended. Mention the filters only if one is the reason for a pushback — they're a checklist for you, not a section of the answer.

## Examples

**Mode A, sound choice:**
User: "I'll use ffuf for directory fuzzing on this target, give me a good command."
→ Give the command. Don't suggest gobuster/feroxbuster as alternatives.

**Mode A, pushback:**
User: "Setting up a Kali VM in VirtualBox just to run nmap and a few CLI tools."
→ Lead with the cost (multi-GB image, RAM, updates, snapshot upkeep for tools that run natively or in a container), then the alternative: install the tools directly via the package manager or run `docker run --rm -it kalilinux/kali-rolling`.

**Mode B:**
User: "I want to step through this heap overflow in gdb instruction by instruction to see how the chunk metadata gets corrupted."
→ Walk through it at that granularity. Don't suggest an automated exploit framework.
