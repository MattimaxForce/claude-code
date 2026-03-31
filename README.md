# Claude Code Source Leak via npm Sourcemap — A Deep Technical Breakdown

> **PS:** Full writeup also available here (better UX & formatting):
> [https://kuber.studio/blog/AI/Claude-Code's-Entire-Source-Code-Got-Leaked-via-a-Sourcemap-in-npm,-Let's-Talk-About-it](https://kuber.studio/blog/AI/Claude-Code's-Entire-Source-Code-Got-Leaked-via-a-Sourcemap-in-npm,-Let's-Talk-About-it)

> There’s a real chance this disappears, so if you care about this kind of thing: archive it, fork it, save it.

---

On March 31st, 2026, Chaofan Shou noticed something unusual in a published npm package.

That “something” turned out to be:

👉 **the complete source code of Claude Code**, the official CLI tool by Anthropic.

Not partial code. Not debug leftovers.

The entire internal codebase — including:

* hidden features
* internal prompts
* experimental systems
* architecture decisions
* unreleased capabilities

All exposed through a **sourcemap file**.

---

## How This Kind of Leak Happens (And Why It Keeps Happening)

Let’s simplify it.

When developers ship JavaScript/TypeScript apps, they often bundle and minify code.

To make debugging possible, they include **source maps** (`.map` files).

These files:

* map compiled code → original source
* allow readable stack traces

But here’s the key detail:

👉 **they often embed the full original source code inside them**

Example:

```json
{
  "sources": ["../src/main.tsx"],
  "sourcesContent": ["// full source code here"]
}
```

That `sourcesContent` field?

👉 That’s literally everything.

---

### Why this leak is not surprising (but still bad)

This exact issue has happened many times before.

Typical causes:

* forgetting to exclude `.map` files in `.npmignore`
* enabling source maps in production builds
* default bundler behavior (Bun does this)

👉 No exploit needed
👉 No hacking required
👉 Just download the package

---

### The irony

Inside the leaked code, there is a feature called:

👉 **Undercover Mode**

Designed to prevent internal data leaks.

But the actual leak happened at the packaging level.

---

## What Claude Code Actually Looks Like Internally

From the outside:

> A clean CLI coding assistant

From the inside:

> A highly complex, modular AI system with orchestration, memory, and automation

---

### Entry point

Main file:

👉 [MAIN](https://github.com/MattimaxForce/claude-code/blob/main/main.tsx)

Size:

👉 ~785KB

That’s extremely large for a CLI entry point.

Which tells us:

👉 most logic is centralized or tightly coupled

---

## BUDDY — A Hidden Virtual Companion System

Located here:
[https://github.com/MattimaxForce/claude-code/tree/main/buddy](https://github.com/MattimaxForce/claude-code/tree/main/buddy)

---

### What it is

A **virtual pet system** inside the terminal.

Not a joke.

---

### Deterministic randomness

Buddy generation uses:

```ts
mulberry32(seed)
```

Seed derived from:

* userId
* constant salt

👉 Result:
Same user → always same pet

---

### Rarity system

| Tier      | Chance |
| --------- | ------ |
| Common    | 60%    |
| Uncommon  | 25%    |
| Rare      | 10%    |
| Epic      | 4%     |
| Legendary | 1%     |

Plus:
👉 1% chance of “shiny”

---

### Generated attributes

Each Buddy has:

* behavioral stats (debugging, chaos, etc.)
* visual traits (eyes, hats)
* ASCII animations
* a generated “personality”

---

### Important takeaway

This is not just UI decoration.

👉 It’s an **interactive personality layer** tied to the assistant

---

## KAIROS — A Proactive Assistant Mode

Located in:
[https://github.com/MattimaxForce/claude-code/tree/main/assistant](https://github.com/MattimaxForce/claude-code/tree/main/assistant)

---

### Core idea

Instead of waiting for input:

👉 Claude watches and acts proactively

---

### How it works

* logs user activity continuously
* processes periodic “ticks”
* decides whether to act

---

### Constraints

Hard rule:

👉 no action should block user workflow > 15 seconds

---

### Why this matters

This is a shift from:

👉 reactive AI → proactive AI systems

---

## ULTRAPLAN — Remote Long-Running Reasoning

One of the most advanced systems found.

---

### What it does

Claude can:

* detect complex tasks
* offload them to remote compute
* run longer reasoning sessions (~30 min)

---

### Flow

1. task identified
2. remote container launched
3. polling every 3s
4. browser UI shows progress
5. user approves result
6. result returns locally

---

### Key detail

Return signal:

```
__ULTRAPLAN_TELEPORT_LOCAL__
```

---

### Insight

This solves a core limitation:

👉 local context + time constraints

---

## DREAM — Background Memory Consolidation

Directory:
[https://github.com/MattimaxForce/claude-code/tree/main/services/autoDream](https://github.com/MattimaxForce/claude-code/tree/main/services/autoDream)

---

### Concept

Claude periodically “reviews” past interactions.

---

### Activation rules

Requires:

* 24h elapsed
* multiple sessions
* lock availability

---

### Process

1. scan memory
2. gather new info
3. rewrite knowledge
4. prune outdated data

---

### Why this is important

This introduces:

👉 **long-term structured memory**

---

## Undercover Mode

File:
[https://github.com/MattimaxForce/claude-code/blob/main/utils/undercover.ts](https://github.com/MattimaxForce/claude-code/blob/main/utils/undercover.ts)

---

### Purpose

Prevent AI from revealing:

* internal info
* model names
* company details
* that it is AI

---

### Implication

Confirms:

* internal usage in public repos
* intentional concealment behavior

---

## Multi-Agent System (Coordinator Mode)

Directory:
[https://github.com/MattimaxForce/claude-code/tree/main/coordinator](https://github.com/MattimaxForce/claude-code/tree/main/coordinator)

---

### What it enables

Claude becomes:

👉 a manager of multiple agents

---

### Workflow

1. research (parallel agents)
2. synthesis (main agent)
3. implementation
4. validation

---

### Key design principle

> parallel execution is essential

---

## System Prompt Architecture

Directory:
[https://github.com/MattimaxForce/claude-code/tree/main/constants](https://github.com/MattimaxForce/claude-code/tree/main/constants)

---

### Structure

Prompt is modular:

* static parts (cacheable)
* dynamic parts (user-specific)

---

### Special function

```ts
DANGEROUS_uncachedSystemPromptSection()
```

Used to force cache invalidation.

---

## Internal API Features (Beta Flags)

File:
[https://github.com/MattimaxForce/claude-code/blob/main/constants/betas.ts](https://github.com/MattimaxForce/claude-code/blob/main/constants/betas.ts)

---

### Examples

* 1M token context
* structured outputs
* advanced tool use
* AFK mode
* thinking redaction

---

### Meaning

👉 API is ahead of public documentation

---

## Tooling System

Directory:
[https://github.com/MattimaxForce/claude-code/tree/main/tools](https://github.com/MattimaxForce/claude-code/tree/main/tools)

---

### Capabilities

* file editing
* shell execution
* web browsing
* task scheduling
* agent spawning

---

### Key insight

Claude is not just a chatbot.

👉 It is a **tool-driven execution engine**

---

## Security & Permissions

Directory:
[https://github.com/MattimaxForce/claude-code/tree/main/tools/permissions](https://github.com/MattimaxForce/claude-code/tree/main/tools/permissions)

---

### Features

* risk classification (low/medium/high)
* protected files
* path traversal prevention
* ML-based approvals

---

### Interesting detail

Even explanations are generated by AI.

---

## Final Thoughts

This leak reveals three important things:

---

### 1. The system is far more advanced than expected

Not just:

* chat
* code generation

But:

* agents
* memory
* autonomy

---

### 2. Many features are not public yet

Including:

* proactive AI
* long-running planning
* persistent assistants

---

### 3. Small mistakes → massive exposure

A single `.map` file exposed everything.

---

---

# VERSIONE ITALIANA 🇮🇹

---

# Il Leak del Codice di Claude Code — Analisi Tecnica Completa

> **PS:** versione completa qui:
> [https://kuber.studio/blog/AI/Claude-Code's-Entire-Source-Code-Got-Leaked-via-a-Sourcemap-in-npm,-Let's-Talk-About-it](https://kuber.studio/blog/AI/Claude-Code's-Entire-Source-Code-Got-Leaked-via-a-Sourcemap-in-npm,-Let's-Talk-About-it)

---

Il 31 marzo 2026, Chaofan Shou ha scoperto qualcosa di enorme:

👉 il codice completo di Claude Code pubblicato accidentalmente su npm.

---

## Come è successo

I file `.map` contengono il codice originale.

Se non vengono esclusi:

👉 diventano accessibili pubblicamente.

---

## Perché è grave

Perché dentro c’era tutto:

* architettura
* prompt
* feature nascoste
* sistemi non rilasciati

---

## Cosa abbiamo scoperto

---

### 1. Sistema multi-agente

Claude può orchestrare più agenti in parallelo.

---

### 2. Memoria persistente (DREAM)

Claude:

* salva informazioni
* le riorganizza
* elimina quelle obsolete

---

### 3. Modalità proattiva (KAIROS)

Claude:

* osserva
* decide quando intervenire
* agisce senza input

---

### 4. Pianificazione remota (ULTRAPLAN)

Task complessi → delegati a container remoti.

---

### 5. Sistema Buddy

Un assistente con personalità:

* stats
* animazioni
* comportamento

---

### 6. Sicurezza avanzata

* classificazione rischio
* protezione file
* prevenzione exploit

---

## Conclusione

Questo leak dimostra:

* I sistemi AI moderni sono molto più complessi di quanto sembri

Il futuro è:

* autonomo
* persistente
* multi-agente

> "Basta un errore banale per esporre tutto"

---

**By MattimaxForce**
