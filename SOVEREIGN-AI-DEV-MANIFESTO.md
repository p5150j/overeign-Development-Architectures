# Sovereign AI Development: From Extraction to Liberation

*A Complete Guide to Breaking Free from Proprietary Coding Agent Ecosystems*

---

# Part I: Why We Build

## We're Already Living All Three
*(The Extraction Economy Didn't Wait for Permission)*

---

### I. The Map Is Wrong

There's a version of this conversation happening everywhere right now, in think tanks and Substacks and conference panels and Discord servers where people who care about the future of AI are trying to map what comes next. The map usually looks something like this:

The extraction economy — the current era of platforms monetizing attention, data, and behavior at industrial scale — is running out of road. AI accelerates and concentrates that extraction to a point where the model breaks. Something has to replace it. The question is what.

Three options get named, in one form or another, depending on who's drawing the map.

The first is **Feudalism**. A handful of platforms own the AI infrastructure. Everyone else gets access as a subscription, a feed, a service. The deal is comfort in exchange for dependency. You don't own anything, you don't control anything, but the UX is smooth enough and the dopamine loop tight enough that most people don't notice or don't care. The cage is ergonomic. This path requires no action — it's the default, the attractor state, what you get if nobody does anything different.

The second is **Communism**. Nations wall off their AI behind digital borders. The open internet splinters into sovereign internets. Every major power races toward AGI in something resembling Manhattan Project secrecy — parallel efforts, zero information sharing, the permanent logic of mutual threat. Fear becomes the operating system. This path doesn't require action either — it requires panic, and panic is always available.

The third is **Shared Ownership**. Community-controlled AI infrastructure. Local cooperatives. Value created in a community stays in that community rather than being extracted upstream to a platform's balance sheet. Not a policy proposal bolted onto the existing architecture but a different architecture entirely. This path requires deliberate construction. It doesn't have gravity on its side.

It's a useful map. It names real forces. The analysis of the first two paths is largely correct, and the argument for why the third path is the only one worth building toward is genuinely compelling.

Here's the problem: the map is drawn in the future tense, and we are not in the future.

These aren't three paths forward. They're three conditions that already exist, running concurrently, right now. The framing of *choice* implies we're standing at a fork before the terrain has been committed to. We're not. We're already deep in all three simultaneously, and most of the proposed solutions are still written as though the divergence point hasn't happened yet.

That gap — between the map and the actual territory — is what this is about.

---

### II. Feudalism Without the Deal

Classic feudalism had a contract. It was exploitative, it was coercive, it was designed entirely to benefit the people at the top of the hierarchy — but it was still a contract. The lord provided something: land access, physical protection, dispute resolution, a social order that kept worse chaos at bay. The serf gave up autonomy and surplus labor in exchange for something they couldn't produce alone. The deal was bad. It was still a deal.

What we have now broke the contract before it was signed.

You pay a monthly subscription to access the tools. The tools process your inputs. Your inputs and their outputs train the next version of the model. The next version gets sold back to you at a higher price point. The model weights belong to the platform. The infrastructure belongs to the platform. The improvements you generated belong to the platform. At no point in this loop does anything flow back to you except continued access — which you are also paying for.

The original pitch for this version of feudalism at least included UBI. Automate the economy, eliminate the jobs, send everyone a check so the consumption loop keeps running. That was the social contract on offer. We didn't get it. The automation is ahead of schedule. The UBI isn't coming. The jobs are going and the subscription fee is due.

This is not a cage with the food included. This is sharecropping with a monthly invoice.

The open source ecosystem is real and it matters and a serious amount of remarkable work is being done inside it. But OSS has become, in practice, the R&D pipeline for the feudal stack more than its alternative. A model gets released, the community fine-tunes it, the techniques get absorbed, the next closed model benefits, and the cycle tightens. The release of weights without the release of training data or compute is a specific kind of openness — open enough to generate community investment, closed enough to retain structural advantage.

And the moment you need scale, the exit closes entirely.

Not because scale is inherently feudal — but because what *counts* as scale was defined by the platforms that built the infrastructure. The benchmarks were written by people who benefit from you needing them to hit the benchmarks. A hundred thousand inference calls a day is fringe by their definition. By any reasonable measure of a functioning local economy, it's substantial. The definition of "production-grade" is not a neutral technical standard. It's a market position.

The compute bill is the clearest version of this. There is no serious AI application at any meaningful throughput that doesn't eventually route money to a handful of infrastructure providers. AWS, Azure, GCP, and a few specialized GPU clouds between them. You can mix and match. You cannot opt out. The hardware itself — the physical GPUs — flows from a supply chain so concentrated it makes the software layer look diverse. One company makes the chips that run the models that run on the clouds that you are billed for monthly.

The tithe went to the church. At least the church built cathedrals you could walk into.

The dissonance that most people building in this space carry around without naming it: you can believe the technology is genuinely powerful and potentially liberatory and still be participating in a system that is structurally extractive at every layer. Those two things are not in contradiction. They are, in fact, the precise combination that makes the system so stable. Useful enough to keep building. Captured enough that building doesn't change the ownership.

That's not pessimism. That's just the architecture.

---

### III. The Cold War That Already Started

Nobody announced it. There was no declaration, no starting gun. That's partly what makes it hard to see — we're looking for a moment that already passed.

The AI cold war didn't start between nations. It started between labs.

Watch the pattern of the last three years. A closed model ships and dominates. An "open" model drops in response — weights released, benchmarks published, community adoption celebrated. The narrative is democratization. The reality is market strategy. Meta releases Llama not because Mark Zuckerberg believes in the commons but because open source commoditizes the layer where OpenAI and Anthropic charge rent. Google releases Gemma. Mistral releases everything. The open source AI ecosystem is real, the research is genuine, the community contributions matter — and the major releases are also calculated moves in a corporate land war dressed up as altruism.

This matters because the OSS AI community largely knows this and participates anyway, because the alternative is having no access at all. That's a rational choice. It's also exactly how you build a commons on someone else's foundation.

And then there's the GPU problem, which is where the cold war framing stops being metaphor and starts being structural.

The models are free. The compute is not. Running anything at scale — fine-tuning, inference, training, even serious evaluation — routes money to a supply chain that terminates in a handful of places. The cloud providers: AWS, Azure, GCP. The specialized GPU clouds. And underneath all of it, one company that manufactures the chips that everything runs on. NVIDIA's market position in AI compute is not a feature of a competitive market — it's a chokepoint. The export controls the US government placed on advanced chips to China weren't a response to the AI race. They were the AI race, expressed as policy, by the people with enough leverage to make hardware availability a geopolitical instrument.

This is what big money and consolidated power actually look like in practice. Not a villain speech. Not a secret meeting. Just the compounding logic of who owns the physical infrastructure that the entire ecosystem depends on, and what happens when that infrastructure becomes the most strategically valuable thing on earth.

The open source model is free. The electricity to run it isn't. The cluster to train it isn't. The cooling system for the data center isn't. The chips aren't. The supply chain for the chips isn't. At every layer below the weights, you hit a wall owned by someone with more capital than any cooperative is currently positioned to match.

That's not an argument against building. It's an argument for being precise about what you're actually building on.

---

### IV. The Third Path, Honestly

Let's start with the people actually trying to build it.

Most serious OSS AI researchers and PhD-level builders fall into one of two groups. Understanding both explains why the third path keeps almost existing and then doesn't.

**Group One** is doing genuinely groundbreaking work — new architectures, new capabilities, things that don't fit the benchmarks because the benchmarks weren't built for what they're solving. The problem isn't the quality of the work. The problem is that taking it from research to real requires massive teams, compute budgets, and distribution infrastructure that no individual or small cooperative can provision. So the big labs swoop in. They offer resources, legitimacy, and compensation that isn't close to what any commons-based structure can match. The researcher joins. The whitepaper lives on. The OSS version goes stagnant. The vision gets built, but it gets built under the company umbrella — which means the weights, the roadmap, and the commercial terms belong to someone else. Noam Shazeer co-authored the Transformers paper, left Google to found Character AI, and was then hired back to Google along with several members of his team in a $2 billion acqui-hire deal. The research happened. The commons didn't get it. Adept's core team, including co-founders who had built genuinely novel agent AI work, joined Amazon's AGI department — and the company's main technology flowed with them. The pattern is consistent enough to be structural.

**Group Two** is fighting the system directly — building OSS versions of paired commercial offerings, releasing weights, publishing code, trying to erode the moat. And they either get a legal notice to shut down, or they start working for a competitor and kill the moat that way. The voice AI space is the clearest current example. ElevenLabs spent years building what looked like a durable position — high-fidelity voice cloning, developer API, market leadership. Then Alibaba's Qwen team dropped Qwen3-TTS: open source, without usage caps, watermarks, or pricing tiers shaping what's possible. Voice cloning from three seconds of reference audio, compared to ElevenLabs' one-to-three minute requirement — and at $0 per character after the initial GPU investment versus $180 per million characters on the commercial platform. The moat evaporated. Not through a competitor building a better commercial product. Through a release that made the entire pricing layer structurally untenable.

This is where the incentive model reveals itself as genuinely broken — and not in the way most people frame it.

The OSS release didn't come from an academic collective or a community cooperative. It came from Alibaba. A company with more capital, more compute, and more strategic interest in commoditizing ElevenLabs' layer than any independent researcher could ever deploy. The "open source wins" story here is actually a cold war story in Group Two clothing. Alibaba didn't release Qwen3-TTS because they believe in the commons. They released it because it advances their position in the infrastructure layer while making a competitor's moat disappear. The community benefits as a side effect of a corporate land grab.

And after all of that — after the acqui-hire absorbs Group One and the strategic OSS release undercuts Group Two — you still need GPUs to run any of it at scale. Self-hosting Qwen3-TTS is free after the initial GPU investment. At ten million characters monthly, ElevenLabs charges $1,800. But that initial GPU investment is real — the 1.7B model requires roughly 16GB of VRAM. The model is free. The hardware isn't. The electricity isn't. The technical expertise to deploy and maintain it isn't. And the supply chain for all of that hardware routes directly back to the same consolidated infrastructure we covered in Section III.

The third path isn't wrong. The vision of community-owned AI infrastructure, local fine-tuning, value retained where it's created — the theory of change is coherent. But the two most common routes into it keep getting absorbed by the same system they were trying to route around, and the physical compute layer remains a wall that no cooperative is currently capitalized to get over.

And even if you cleared the compute wall — even if the GPUs were available, even if the models were good enough for the specific community use case — you still need the team. Not moonlighters. Not part-timers squeezing in hours after the kids are in bed. Full dedicated teams running real sprints with real accountability, which means full salaries, which means capital, which means you're back at the same chokepoint dressed in different clothes. The median mortgage in a major tech hub is north of $500k. Cars cost what they cost. Netflix renews whether or not your pull request merged. The personal economics that make the extraction economy feel inescapable are the same economics that make collective infrastructure building structurally impossible for the people most motivated to do it. You can't build the alternative on stolen time. The system knew that when it priced everything the way it did.

That's the gap. Not an argument against building. An argument for being precise about what "building the third path" actually requires — and how far we currently are from having it.

---

### V. Outputs Decide What Counts as Proof

Here's what nobody building in the third path wants to say out loud: without real capital, real teams, and real infrastructure running on real sprints with real KPIs, what you ship is a cool toy.

Not worthless. Not unimpressive. A cool toy.

And cool toys don't replace infrastructure. They don't get plugged into hospital systems or municipal networks or supply chains. They don't run at the reliability, latency, and support tier that any serious operator requires before they stake their business on it. The consumer doesn't care about your values or your architecture or how clean your cooperative bylaws are. They care whether it works when they need it to, at the quality level they've been trained to expect by the products that had hundreds of millions of dollars to get there first.

This isn't the Linux era. That comparison gets made constantly and it's wrong in ways that matter. Linux won because it could be built incrementally, deployed on existing commodity hardware, and improved through distributed contribution over years without requiring any single entity to coordinate massive simultaneous investment. The stack rewarded patience and modularity. If your kernel had a bug, you patched the kernel. The rest kept running.

AI doesn't work like that. A model isn't a kernel. You can't patch one layer while the rest holds. Quality is systemic — it requires training data at scale, compute at scale, RLHF pipelines, evaluation infrastructure, red-teaming, deployment tooling, and ongoing fine-tuning, all coordinated, all funded, all running continuously. The gap between "impressive demo" and "production system" isn't a gap you close with a few more contributors and a weekend sprint. It's a capital problem dressed up as a technical one.

And the walled gardens aren't a bug in this picture — they're the point. The reason the feudal stack is so sticky isn't just that it's well-funded. It's that the walls create the quality. The closed training data, the proprietary RLHF, the internal safety systems, the deployment infrastructure — none of that is accessible to a cooperative. You can't fork it. You can't replicate it on a shoestring.

You can run a local model that does 55% of what the frontier model does, and for a narrow use case that might be enough. But the consumer has already been anchored to 70% — and nobody's even hit 100% yet. The frontier keeps moving, the anchor moves with it, and the gap between what a well-resourced lab ships next quarter and what a cooperative can build this year isn't closing. It's compounding. The 15-point gap is where trust lives, and trust is what determines whether something gets adopted at scale or stays in the enthusiast tier forever.

The third path has a serious answer to this or it doesn't have an answer. Inspiration and architecture aren't enough. Someone has to fund the sprints.

---

### VI. Who Builds Anyway

You've read this far. You already know which side you're on.

You're not waiting for permission. You're not waiting for the incentive model to make sense, for the conditions to be fair, for someone with a bigger platform to say it's time. You already know it's time. You've known for a while. The question was never whether — it was whether enough other people would stop scrolling long enough to give a damn.

Here's the truth: the feudal stack is counting on your exhaustion. It is designed to be exhausting. The subscription renews automatically. The benchmark moves just fast enough to keep the gap demoralizing. The acqui-hire comes in right when something starts to matter. The system doesn't need to defeat you. It just needs you to decide the odds are too long and go back to building on top of it instead of against it.

Don't.

Not because the odds are good — they're not. Not because the capital is coming — it might not. Not because the walled gardens are about to fall — they're not falling today. Build because the only thing that has ever broken consolidated power is people who refused to wait for better conditions and built the alternative in the worse ones. Every cooperative, every commons, every piece of infrastructure that ever escaped extraction economics was built by people who were told the timing was wrong and the resources weren't there and the incumbents were too strong.

They were right. Do it anyway.

The third path doesn't need believers. It needs builders. It needs people who are done performing outrage on platforms that extract the outrage and sell it back as engagement. It needs people who will take the skills, the time, and the fury they've been pouring into the feudal stack and redirect even a fraction of it toward something that doesn't have a terms-of-service clause that owns the output.

And here's the part nobody puts in the manifesto: the train has already left. Not metaphorically. The capital is deployed, the infrastructure is locked, the models are trained, the distribution is captured. This didn't happen while you were sleeping — it happened while you were working, paying rent, raising kids, doing the things that keep a life from falling apart. You were busy surviving the extraction economy while the extraction economy was busy becoming permanent.

Collectively, we had a window. It required coordination, capital, and political will at a moment when most people were still figuring out what a large language model even was. That window is not open in the same way anymore.

So yes. You're fucked. Your kids are inheriting this. And unless you want to raise them in a world where every thought they have, every skill they build, every piece of value they create gets routed through infrastructure they will never own and can never leave — someone has to build the alternative anyway. Knowing the odds. Knowing the cost. Knowing the train is already gone and you're building the next track.

That's not inspiration. That's the actual ask.

Find your people. Build the seed. Document everything. Make it so good and so undeniable that the next person doesn't have to start from zero.

The revolution doesn't get announced. It gets committed.

---

# Part II: What We're Building Against

## Sovereign Development Architectures
*Evaluating Production-Grade Open Source Alternatives to Proprietary Coding Agent Ecosystems*

---

The software engineering landscape in 2026 is defined by a fundamental transition from simple, predictive autocompletion to fully autonomous agentic workflows. At the vanguard of this shift is Anthropic's Claude Code, a terminal-native execution agent that has redefined the expectations for context-aware development. However, the prohibitive costs of the $200 per month Max plan, combined with inherent concerns regarding data sovereignty and token-based usage constraints, have catalyzed a professional movement toward high-performance open-source (OSS) replacements. For developers and enterprises seeking to replicate or exceed the capabilities of the Claude ecosystem within a self-hosted or provider-agnostic framework, the selection of the underlying large language model (LLM) and the agentic scaffolding is a critical architectural decision.

### The Fiscal and Technical Constraints of Proprietary Coding Subscriptions

The adoption of Claude Code is frequently driven by its state-of-the-art reasoning capabilities, particularly when powered by the Opus 4.6 model family. Nevertheless, the subscription model employed by Anthropic introduces several friction points for professional workflows. The $200 per month Max 20x plan is marketed as providing twenty times the usage of the standard Pro plan, yet this is often an abstraction of a complex token-based limit system. Independent technical analysis suggests that these limits translate to approximately 220,000 tokens per session for the highest tier, a ceiling that intensive agentic loops—which involve repeated planning, execution, and testing cycles—can reach within minutes.

| Subscription Tier | Monthly Cost | Stated Usage Limit | Effective Session Capacity |
|-------------------|--------------|--------------------|-----------------------------|
| Free | $0 | Minimal | Insufficient for agentic loops |
| Pro | $20 | Baseline | 10-40 prompts per 5 hours |
| Max 5x | $100 | 5x Pro | 50-200 prompts per 5 hours |
| Max 20x | $200 | 20x Pro | 200-800 prompts per 5 hours |

Beyond the financial outlay, the architectural rigidity of proprietary tools introduces operational risks. For instance, Claude Code requires a continuous internet connection and sends all project metadata to Anthropic's servers, creating potential compliance conflicts for organizations governed by SOC2 or HIPAA standards. Furthermore, internal leaks of the Claude Code source code have revealed a surprisingly complex and sometimes inefficient internal architecture, characterized by deeply nested React components and experimental features like "Undercover Mode" and a "Companion Pet System" that may distract from core engineering utility.

### The Benchmark Landscape: Reasoning Parity in 2026

To serve as a viable replacement for the Claude ecosystem, an open-source model must demonstrate mastery over the two dominant benchmarks of modern code intelligence: HumanEval, which measures single-function synthesis, and SWE-bench, which evaluates the resolution of real-world GitHub issues. While Claude Opus 4.5 and 4.6 remain benchmarks for reasoning depth, scoring upwards of 80.8% on SWE-bench Verified, the gap between proprietary and open-weight models has narrowed significantly.

#### Performance Metrics of Frontier Open-Weight Models

As of early 2026, several open-weight models have emerged as production-grade contenders. The Qwen 3 series from Alibaba Cloud, alongside DeepSeek's V4 models, provide the backbone for most high-performance OSS stacks. These models leverage advanced architectures such as Mixture-of-Experts (MoE) and hybrid "Thinking Modes" to maintain high reasoning quality.

| Model Variant | HumanEval Score | SWE-bench Verified | Architecture Type |
|---------------|-----------------|--------------------|--------------------|
| Claude Opus 4.6 | 97.0% | 80.8% | Proprietary Dense |
| Qwen 3 Coder 32B | 91.2% | ~78% (Est.) | Open MoE |
| DeepSeek V4 | 90.0% (Rumored) | 80%+ (Rumored) | Open MoE + Engram |
| GLM-5 | 90.0% | 77.8% | Open MoE |
| Kimi-Dev-72B | 99.0% | 60.4% | Open Dense |

### Infrastructure Requirements: The RTX 5090 Advantage

A primary motivation for replacing the $200 Claude Max plan is the desire for predictable costs and offline capability. Achieving parity with cloud-based performance is fundamentally gated by Video Random Access Memory (VRAM) and memory bandwidth.

#### The 32GB VRAM Threshold

The release of the **NVIDIA RTX 5090** in January 2025 has redefined the possibilities for local production setups. With **32 GB of GDDR7 VRAM** and a bandwidth of **1.79 TB/s**, it provides a 78% improvement in memory speed over the previous generation. For coding agents, this unlocks several high-tier configurations:

- **Native Precision Models:** The 5090 can run 14B-32B parameter models like Qwen 3 Coder at full precision (BF16) or high-fidelity Q8_0 quantization without "spilling" into slower system RAM.

- **Extensive Context Windows:** While a 4090 often struggles to maintain a 128K context for 32B models, the 5090 supports full **256K token context windows** at interactive speeds (30-40 tokens per second).

- **Throughput Gains:** The 5090 delivers 30-40% better AI throughput than the 4090, which is critical for agentic loops that require the model to "think," generate code, and self-correct multiple times per request.

| Hardware Tier | Recommended Components | Supported Models | Context Limit |
|---------------|------------------------|------------------|---------------|
| **Professional** | RTX 4090 24GB | 14B-32B (Q4-Q5 Quant) | 32K - 64K |
| **Enthusiast** | RTX 5090 32GB | 14B-32B (Q8/BF16) | 128K - 256K |
| **Workstation** | Mac Studio M4 Ultra | 70B+ models | 512K+ |

### Scaffolding the Sovereign Stack: Agentic Frameworks

A high-performance model is only one component of a successful Claude Code replacement. The "scaffolding" transforms the LLM into a coding agent.

#### Aider: The Git-Native Paradigm

Aider remains the gold standard for git-heavy pair programming. It utilizes a "Repo Map" to manage context, allowing it to reason about cross-file dependencies in large repositories without indexing every token. On an RTX 5090, Aider can leverage models like **DeepSeek R1** or **Qwen 3 Coder** to perform autonomous, multi-file "surgical" edits with immediate git-commit traceability.

#### OpenCode: The Polished Terminal IDE

OpenCode is a rapidly growing alternative (100K+ stars) that emphasizes provider agnosticism. It supports over 75 LLM backends and features a dual-agent architecture with a "Build" agent for writing and a "Plan" agent for analysis. Its native Language Server Protocol (LSP) integration allows it to detect syntax errors in real-time, enabling "self-healing" code loops.

#### Goose: The Autonomous System Architect

Originally developed by Block (formerly Square), Goose focuses on high-level system design. It emphasizes a "planning-first" approach where the agent breaks requests into verifiable steps before execution. It is specifically noted for its integration with the Model Context Protocol (MCP), allowing it to connect to external databases and tools.

### Fiscal Analysis: Subscription vs. Local Hardware

The $2,400 annual cost of the Claude Max plan is the baseline for total cost of ownership (TCO) comparisons.

- **Local Setup (RTX 5090):** A complete enthusiast workstation costs approximately **$5,000–$8,000**. While the upfront cost is high, it eliminates all subscription and per-token fees, reaching a financial break-even point against a Max 20x subscription in roughly 24–36 months—not accounting for the added value of zero-latency and total privacy.

- **API Gateway (OpenRouter/Together AI):** For those avoiding hardware investment, models like Qwen 2.5/3 are available at rates as low as $0.09 per million tokens. An active developer typically spends $2–$5/month on APIs using this model, representing a **97% saving** over the Claude Max plan.

### Conclusion: Implementing the Optimal Replacement Strategy

For a developer with an RTX 5090, the optimal Claude Code replacement strategy is a **fully local, high-fidelity stack**. By pairing an agentic framework like **Aider** or **OpenCode** with a **Q8-quantized Qwen 3 Coder 32B** or the upcoming **DeepSeek V4**, you can achieve 90-95% of the performance of the Claude Max plan with 256K tokens of local context, zero marginal cost, and complete data sovereignty.

---

# Part III: How to Build It

## Free Agentic Coding: Replacing Claude Code in 2026

**You can now run a genuinely capable AI coding agent in VS Code for $0 per month.** The combination of Roo Code (or Cline) with Qwen3.5-27B running locally via Ollama delivers roughly **72% on SWE-bench Verified** — within striking distance of Claude Code's proprietary models at 80%. The gap has shrunk from insurmountable to manageable, especially for day-to-day coding. The real breakthrough is MoE architecture: Qwen3.5-35B-A3B achieves **69.2% SWE-bench with just 8GB VRAM**, making production-grade agentic coding accessible on modest hardware. Below is a complete guide to the best tool + model combinations, benchmarks, and practical setup advice.

---

### The VS Code Agentic Tools Worth Your Time

Four open-source tools have emerged as genuinely production-ready for agentic coding in VS Code. All are Apache 2.0 licensed and support local models via Ollama or any OpenAI-compatible API.

**Roo Code** (22K GitHub stars, 1.5M+ users) is the most feature-rich option. Forked from Cline, it adds a **Custom Modes system** (Architect, Code, Debug, Orchestrator, plus user-defined personas), diff-based editing that saves tokens compared to whole-file rewrites, inline autocomplete, a Memory Bank for persistent context across sessions, and browser automation. It's SOC 2 Type 2 compliant and ships multiple releases per week. The community consistently calls it a "superset of Cline." It works in VS Code, VSCodium, and even inside Cursor and Windsurf.

**Cline** (58K GitHub stars, 5M+ installs) pioneered the IDE-native agentic approach. Its Plan/Act dual-mode workflow lets you explore solutions before committing changes. Key advantages include an **MCP Marketplace** for one-click tool integration, native subagents (v3.58) for parallel task decomposition, and browser automation for debugging UIs. Cline's "compact prompt" mode reduces the system prompt by 90% — critical when running local models with limited context. One caveat: Cline Bot Inc.'s Terms of Service created controversy around data rights, despite the Apache 2.0 license on the code itself.

**Aider** (39K GitHub stars, 4.1M installs) is the terminal-first option and remains the gold standard for git-native workflows. Every AI edit becomes a clean, auditable git commit. Its tree-sitter-based repository mapping gives it **superior structural understanding** of codebases across 100+ languages. Aider maintains its own model leaderboard at aider.chat, making it easy to compare local models. It lacks a VS Code extension but works seamlessly from the integrated terminal via "watch mode." Best for developers who prefer CLI workflows and want maximum safety through version control.

**Continue.dev** (32K GitHub stars) takes a different approach, excelling at **inline autocomplete and chat** rather than full autonomy. Its Agent Mode handles multi-step tasks, but the community describes it as "more copilot than autonomous agent." The real strength is privacy-first architecture — it runs completely air-gapped with Ollama — and dual IDE support (VS Code and JetBrains). Best used alongside a more agentic tool: Continue for fast autocomplete, Cline or Roo Code for complex tasks.

| Feature | Roo Code | Cline | Aider | Continue.dev |
|---------|----------|-------|-------|-------------|
| VS Code native extension | Yes | Yes | No (terminal) | Yes |
| Multi-step autonomous tasks | Yes | Yes | Yes | Yes (limited) |
| Terminal command execution | Yes | Yes | Yes (native) | Yes |
| Browser automation | Yes | Yes | No | No |
| Custom agent modes | Yes (5+ built-in) | Plan/Act only | Chat modes | 4 modes |
| Inline autocomplete | Yes | No | No | Yes |
| Git auto-commit | Basic | Basic | Yes (deep) | Basic |
| Diff-based editing (token-efficient) | Yes | No (whole-file) | Yes | Yes |
| Ollama / LM Studio / vLLM | Yes | Yes | Yes | Yes |
| MCP support | Yes (manual) | Yes (marketplace) | No | Yes |

**Tools to skip:** Void Editor has **paused development** and may never resume. Plandex's cloud service shut down in October 2025; self-hosted mode exists but momentum has died. Cursor's free tier is too limited for serious agentic work. OpenHands and Goose are excellent platforms but are standalone tools (CLI/web), not VS Code extensions — consider them if you want GitHub-issue-to-PR automation rather than interactive coding.

---

### Which Open-Source LLM to Run Locally

The model landscape has transformed. Qwen's family dominates local coding, with multiple models hitting **70%+ SWE-bench Verified** while fitting on consumer GPUs. Here are the models that matter, ranked by practicality.

**Qwen3.5-27B** is the top recommendation for anyone with a 24GB GPU. This dense 27B model scores **72.4% on SWE-bench Verified** — tying GPT-5 mini — and fits comfortably at Q4_K_M quantization (~16–20GB VRAM). It's Apache 2.0 licensed with no commercial restrictions. Install with `ollama run qwen3.5:27b`. This is the single best balance of quality, speed, and hardware requirements available today.

**Qwen3.5-35B-A3B** is the efficiency miracle. Despite being a 35B-parameter model, its MoE architecture activates only **3B parameters per token**, requiring just ~8GB VRAM. It scores **69.2% on SWE-bench Verified** — remarkable for hardware that costs under $200. This model makes agentic coding viable on an RTX 3060 or a MacBook with 16GB RAM. Install with `ollama run qwen3.5:35b-a3b`.

**Devstral Small 2 (24B)** from Mistral scores **68.0% on SWE-bench Verified** and was purpose-built for agentic coding tasks. Apache 2.0 licensed, it fits on a single RTX 4090 and has excellent tool-calling capabilities. It pairs naturally with Mistral's open-source Vibe CLI but works equally well with Cline or Roo Code.

**Qwen2.5-Coder-32B** remains the gold standard for pure code generation and completion (matching GPT-4o on HumanEval at ~92%), though it predates the SWE-bench agentic era. At Q4_K_M (~20GB VRAM), it's the model most tested and validated by the Aider community, scoring **72.9% on Aider's own benchmark**. If you want the most battle-tested local coding model, this is it.

For **budget hardware** (8–16GB VRAM), the hierarchy is clear: Qwen3.5-35B-A3B (69.2% SWE-bench, ~8GB) > Qwen2.5-Coder-14B (~9GB, solid) > Qwen2.5-Coder-7B (~5GB, autocomplete only). DeepSeek-R1-Distill-32B (~20GB) is worth keeping for hard debugging sessions where deep reasoning matters, but it's slow and not a coding specialist.

**Models to avoid for local use:** Llama 4 variants are general-purpose multimodal models that underperform dedicated coding models significantly. Codestral 25.01 has a non-commercial license. The full DeepSeek-V3 series (671B) requires datacenter hardware.

#### SWE-bench Verified Scores for Locally-Runnable Models

| Model | SWE-bench Verified | VRAM (Q4) | License |
|-------|-------------------|-----------|---------|
| **Qwen3.5-27B** | 72.4% | ~16–20GB | Apache 2.0 |
| Qwen3-Coder-Next (~3B active) | >70% | ~8GB | Apache 2.0 |
| **Qwen3.5-35B-A3B** | 69.2% | ~8GB | Apache 2.0 |
| Devstral Small 2 (24B) | 68.0% | ~16GB | Apache 2.0 |
| Qwen3.5-122B-A10B | 72.0% | ~48GB | Apache 2.0 |
| DeepSeek-V3.1 (37B active/671B) | 66.0% | Datacenter | DeepSeek License |
| *Claude Opus 4.6 (proprietary)* | *80.8%* | *Cloud only* | *Proprietary* |

The gap between the best local model (72.4%) and Claude Code's backbone (80.8%) is **about 8 percentage points**. In practice, this gap matters most for complex multi-file architectural refactors and subtle logic bugs. For everyday coding — writing functions, fixing bugs, generating tests, scaffolding features — local models are genuinely competitive.

---

### Hardware Reality Check and Setup Guide

The hardware sweet spot is an **RTX 4090 (24GB VRAM)**, which runs Qwen3.5-27B or Qwen2.5-Coder-32B at full Q4 precision with room to spare. Apple Silicon users need an M2/M3 Max with 32GB+ unified memory. The MoE revolution means even an **RTX 3060 (12GB)** can run Qwen3.5-35B-A3B at 69.2% SWE-bench quality.

**Setup A — Best quality, 24GB GPU ($0/month):**
```bash
ollama pull qwen3.5:27b
# Install Roo Code from VS Code marketplace
# Settings → Provider: Ollama → Model: qwen3.5:27b
```

**Setup B — Budget hardware, 8-12GB GPU ($0/month):**
```bash
ollama pull qwen3.5:35b-a3b
# Install Cline from VS Code marketplace
# Enable "Compact Prompt" mode (critical for local models)
# Settings → Provider: Ollama → Model: qwen3.5:35b-a3b
```

**Setup C — Terminal power user ($0/month):**
```bash
pip install aider-chat
ollama pull qwen2.5-coder:32b
aider --model ollama/qwen2.5-coder:32b
```

**Setup D — Hybrid approach (~$10–30/month):**
Use Continue.dev + Ollama + Qwen2.5-Coder-7B for instant autocomplete (free). Use Roo Code + Qwen3.5-27B locally for standard agentic tasks (free). Fall back to DeepSeek V3.2 API ($0.27/M input tokens) via Cline for complex multi-file refactors that exceed local model capabilities. This hybrid approach captures **90% of Claude Code's utility at 10–15% of the cost**.

Setup complexity is real but manageable. Expect **30–45 minutes** for first-time Ollama + model download + extension configuration, versus 2 minutes for Claude Code. The `Q4_K_M` quantization format offers the best quality-to-memory tradeoff. Disable KV Cache Quantization in LM Studio if using it. Flash Attention should be enabled for speed.

---

### What Local Setups Still Can't Do as Well as Claude Code

Honesty matters here. **Local models fail more often on three categories of tasks:** complex multi-file architectural refactors requiring coherent changes across 10+ files, subtle security-sensitive code where hallucinated logic can introduce vulnerabilities, and very long context sessions where performance degrades as the context window fills past ~100K tokens.

Speed is noticeably slower. Local inference delivers **20–40 tokens/second** on consumer GPUs versus 50–100+ tokens/second from cloud APIs. The first prompt in a session is especially sluggish while the model loads into VRAM. Token efficiency is also worse — one tester found three small tasks (~200 lines total) consumed nearly 2 million tokens with a local model, compared to far less with Claude.

The practical workaround, endorsed by most experienced users, is the hybrid approach: **run local for the 80% of tasks that are straightforward** (function generation, test writing, bug fixes, boilerplate, documentation) **and route the hard 20% to a cheap cloud API** like DeepSeek V3.2 at $0.27 per million input tokens. A $500 GPU pays for itself versus $200/month Claude Code in under 3 months, and you own the hardware forever.

---

### The Recommended Stack

For most developers seeking to replace Claude Code, the clear winner is **Roo Code + Qwen3.5-27B via Ollama** on a 24GB GPU. This delivers the best VS Code integration, the most powerful agentic features (Custom Modes, diff-based editing, Memory Bank, browser automation), and the highest SWE-bench score achievable on consumer hardware — all at zero ongoing cost under a fully permissive Apache 2.0 license.

If budget hardware is your constraint, **Cline + Qwen3.5-35B-A3B** on 8GB VRAM is a genuine game-changer — 69.2% SWE-bench from a model that fits on a laptop GPU was unthinkable a year ago. For terminal-first developers, **Aider + Qwen2.5-Coder-32B** provides the safest workflow via automatic git commits and the most battle-tested local model integration.

The landscape is moving fast. Qwen3.5 launched in February 2026 and immediately obsoleted many previous recommendations. Devstral Small 2, Qwen3-Coder-Next, and the MoE efficiency revolution mean the next generation of models will likely close the remaining 8-point SWE-bench gap with proprietary models. The free and open-source agentic coding stack isn't just viable — it's rapidly becoming the rational default for cost-conscious developers.

---

# Appendix: References

1. Best AI Coding Tools 2026: Complete Ranking by Real-World Performance | NxCode
2. Claude Code costs up to $200 a month. Goose does the same thing... | VentureBeat
3. Stop Paying Anthropic $200/month for Claude Code (Do This Instead) | YouTube
4. Introducing Claude Opus 4.6 | Anthropic
5. What is the Max plan? | Claude Help Center
6. OpenClaw vs Claude Code vs OpenCode: 2026 Ultimate Guide | GlobalGPT
7. Claude Pricing Explained: Subscription Plans & API Costs | IntuitionLabs
8. Claude Pricing in 2026 for Individuals, Organizations, and Developers | Finout
9. OpenCode vs Claude Code: Which Agentic Tool Should You Use in 2026? | DataCamp
10. $340 billion Anthropic source code leaked | Times of India
11. Anthropic's AI coding tool accidentally reveals its source code | LiveMint
12. HumanEval: LLM Code Synthesis Benchmark | Emergent Mind
13. 15 LLM coding benchmarks | Evidently AI
14. We Tested 15 AI Coding Agents (2026). Only 3 Changed How We Ship | Morph
15. Codestral 22B, Qwen 2.5 Coder B, and DeepSeek V2 Coder | Deepgram
16. Best LLM for Coding 2025: Top Open Source and Paid AI Models | Openxcell
17. Qwen3: Think Deeper, Act Faster | Qwen
18. Comparative Analysis of LLMs | Falah Gatea
19. DeepSeek V4 1 Trillion Parameters | Reddit
20. DeepSeek V4 Targets Coding Dominance | Introl
21. Self-Hosted AI for Developers: Best Coding LLMs in 2026 | DEV Community
22. Ollama VRAM Requirements: Complete 2026 Guide | LocalLLM
23. Self-Hosted AI Models: A Practical Guide to Running LLMs Locally (2026) | Prem AI
24. Hardware requirements to run qwen 2.5 32B? | Reddit
25. Qwen2.5-32B: Specifications and GPU VRAM Requirements | ApX Machine Learning
26. Aider vs OpenCode vs Claude Code vs Goose: CLI AI Assistants | sanj.dev
27. Aider vs OpenCode: Best Open-Source AI Coding CLI in 2026 | NxCode
28. 9 Best AI Coding Agents in 2026, Ranked | MightyBot
29. Best Open Source AI Coding Agents 2026: Tested & Reviewed | CSS Author
30. The Best Coding AI Tools for 2026 | Medium
31. DeepSeek V3 vs Qwen2.5-Coder 32B Instruct | AnotherWrapper
32. Qwen2.5 Coder 3B API Pricing 2026 | PricePerToken
