# Awesome Agentic Bug Hunting

A curated list of public resources about using coding agents, LLM-driven harnesses, skills, and repeatable workflows to find and validate real software vulnerabilities.

The focus is practical agentic bug hunting: source-code or binary exploration, vulnerability hypothesis generation, validation, PoC construction, triage, and repeatable harness design.

Last reviewed: 2026-07-09

## Contents

- [About this list](#about-this-list)
- [Articles](#articles)
- [Projects](#projects)
- [Papers](#papers)

## About this list

**What to include.** A resource should show at least one of the following:

- A concrete LLM- or agent-driven vulnerability discovery workflow.
- A reusable harness, scaffold, agent skill, agent architecture, or validation loop.
- A case study where an agent helped find, validate, reproduce, or report real vulnerabilities.
- A research method where the LLM directly participates in discovery, hypothesis generation, validation, or specification inference.

## Articles

- [Anthropic — Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
  General long-running agent harness design: decomposed tasks, structured handoff artifacts, generator/evaluator separation, and resets for maintaining coherence over multi-hour autonomous work.

- [Anthropic — Mythos: Previewing a Frontier Vulnerability Discovery System](https://www.anthropic.com/research/mythos-preview)
  Describes the Mythos scaffold: isolate the source, ask an agent to inspect code, generate hypotheses, run the program, confirm or reject findings, and output reports with PoCs. Also describes file ranking and parallel agent passes.

- [Anthropic — AI agents find $4.6M in blockchain smart contract exploits](https://www.anthropic.com/research/smart-contracts)
  Smart-contract agent study with a sandboxed local-fork environment and MCP-exposed tools. Kept despite the benchmark framing because it includes novel previously unknown smart-contract vulnerabilities and concrete lessons about executable exploit validation.

- [OpenAI — Harness Engineering](https://openai.com/index/harness-engineering/)
  General agent-first engineering lessons: make repositories legible to agents, encode constraints mechanically, expose validation signals, and design feedback loops so agents can perform reliable long-running work.

- [OpenAI — Codex Security](https://openai.com/index/codex-security-now-in-research-preview/)
  System writeup for an application-security agent that builds project context and threat models, validates findings in sandboxed environments where possible, proposes patches, and reports concrete OSS vulnerability examples.

- [Google Project Zero — Project Naptime](https://projectzero.google/2024/06/project-naptime.html)
  Research prototype for LLM-assisted vulnerability research. Important for its agent interface design: code browsing, debugging, running experiments, and iterative reasoning.

- [Google Project Zero — From Naptime to Big Sleep](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html)
  Follow-up on moving from a research prototype toward a more capable vulnerability-research agent, with a useful trajectory of hypothesis formation, failed testcase adaptation, crash reproduction, and root-cause reporting.

- [Cloudflare — Build Your Own Vulnerability Harness](https://blog.cloudflare.com/build-your-own-vulnerability-harness/)
  A practical walkthrough on building a vulnerability harness around frontier models: staged persistence, dynamic threat modeling, per-task sandboxes, wishlist/resource handoff, cross-repo tracing, dedupe, and validation gates.

- [Team Atlanta — From Harness to Vulnerability: AI Agents for Code Comprehension and Bug Discovery](https://team-atlanta.github.io/blog/post-mlla-disc-agents/)
  First-place AIxCC team deep dive on the quiet discovery agents behind Atlantis: harness entrypoint analysis, precise function retrieval, recursive call-graph construction, tainted-argument tracking, and sink annotation before exploit generation. Useful because it explains how to turn a huge repo into focused vulnerability leads instead of relying on broad prompts.

- [Trail of Bits — Buttercup is now open-source!](https://blog.trailofbits.com/2025/08/08/buttercup-is-now-open-source/)
  Second-place AIxCC team's open-source release note with concrete architecture: AI-augmented vulnerability discovery, contextual static analysis, PoV deduplication, and seven-agent patch generation/validation. Kept as the most useful Buttercup overview; older pre-final AIxCC posts are intentionally excluded.

- [Xint — Building Effective LLM Agents | AI Cyber Challenge](https://xint.io/blog/building-effective-llm-agents-ai-cyber-challenge-165236)
  Agent-design lessons from Theori's AIxCC work: decompose vulnerability discovery, PoV generation, root-cause analysis, and patching into narrower agents; curate tools instead of exposing raw bash; and force structured outputs that include trigger conditions.

- [Xint — Copy Fail: 732 Bytes to Root on Every Major Linux Distribution](https://xint.io/blog/copy-fail-linux-distributions)
  High-signal kernel case study: a human researcher supplied the key attack-surface observation, then Xint Code scaled the search across Linux crypto paths and surfaced CVE-2026-31431. Useful for learning how operator prompts can encode one sharp invariant and let an agent search the combinatorial space around it.

- [AISLE — System Over Model: Zero-Day Discovery at the Jagged Frontier](https://aisle.com/blog/system-over-model-zero-day-discovery-at-the-jagged-frontier)
  Concrete counterpoint to frontier-only thinking: a simple parallel scanner generates per-file security context, scans for C/C++ memory-safety bugs, then runs skeptical multi-round triage. Useful for learning how coverage, cheap models, and triage can surface real kernel/native bugs.

- [Microsoft — MDASH](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/)
  Closed but unusually concrete writeup of a multi-model agentic scanning harness: prepare, scan, validate, dedupe, prove, and patch-oriented stages with auditor, debater, prover, and domain-plugin roles.

- [ClickHouse — How I Hunt for Vulnerabilities with AI](https://clickhouse.com/blog/how-i-hunt-for-vulnerabilities-with-ai)
  Tsvetan Stoychev describes using LLMs to navigate a large C++ codebase, generate hypotheses, inspect dataflow, validate locally, and write reports. Valuable because it focuses on the human-in-the-loop workflow rather than full autonomy.

- [Praetorian — AI Vulnerability Research in the FreeBSD Kernel](https://www.praetorian.com/blog/ai-vulnerability-research-freebsd-kernel/)
  A kernel-focused case study using agentic coding tools, custom prompts, skills, and KASAN-backed validation loops. Useful for seeing how agentic research changes the speed of auditing and vulnerability discovery without removing expert review.

- [Security Cryptography Whatever — AI Bug Finding (with Nicholas Carlini)](https://securitycryptographywhatever.com/2026/03/25/ai-bug-finding/)
  Podcast conversation on systematically finding bugs with Claude Code — auditing codebases with plain “find a bug” prompts and disclosing hundreds of real vulnerabilities to projects like Firefox. Useful for how a practitioner frames discovery, triage, and disclosure at scale.

- [Monad — Monad Bugfinder](https://www.monad.xyz/blog/monad-bugfinder)
  A practical bug-hunting workflow inspired by Anthropic Mythos and TxRay. The design separates discovery from validation, uses a structured database as the source of truth, and treats triage as a different task from finding leads.

- [Gustavo Grieco — Introducing Quimera](https://gustavo-grieco.github.io/blog/introducing-quimera/)
  Feedback-driven exploit-generation loop for Ethereum contracts using source/on-chain context and Foundry traces. Useful mainly for the insight that LLMs perform best when the loop has an executable success signal and tight failure feedback.

- [Yue Xue — AI Auditing Methodology: Agent Self-Evolution, Drift, Reverse Evolution and Solutions](https://www.linkedin.com/pulse/ai-auditing-methodology-agent-self-evolution-drift-reverse-yue-xue-uqsse/)
  Field notes on why long-running audit agents drift, and how to control them by decomposing the project, defining narrow tasks, and reasoning over entrypoints, assets, state, trust boundaries, and invariants.

- [Yue Xue — AI Auditing Methodology, Part I](https://x.com/xy9301/status/2033186266649640980)
  First installment of the same smart-contract auditing methodology series, linked from Alin-Mihai BARBATEI's post.

- [Yue Xue — AI Auditing Methodology, Part II](https://x.com/xy9301/status/2036017855381340269)
  Second installment of the same smart-contract auditing methodology series.

- [pkqs91 / Octane Security — How I Made $200k With Codex in 3 Months](https://x.com/pkqs91/status/2070157806104457395)
  A widely shared bug-bounty field note. The durable lesson is not “ask an agent to find bugs”, but to build a harness around recon, context-building, attack-surface mapping, lead generation, validation, PoC writing, and reporting.

- [Plamen Tsanev — BEAST post](https://x.com/p_tsanev/status/2041880807032119670)
  Original field note behind the Plamen orchestrator. Useful for understanding the motivation behind multi-worker Web3 audit orchestration.

- [Zero Cool Labs — Symmetry Sniper](https://x.com/ZeroCool_AI/status/2069443859327705497)
  Public note about a narrow Web3 agent skill focused on symmetry-style bug discovery. Useful as an example of building small, specialized security skills instead of one broad auditor.

- [Alin-Mihai BARBATEI — Notes on building a smart contract security auditing harness](https://www.linkedin.com/posts/alin-mihai-barbatei-27772b54_notes-on-building-a-smart-contract-security-activity-7476636721527455744-favn)
  A compact reading list around agentic smart-contract auditing. The useful taxonomy is: discovery/recon, lead generation, finding identification, issue validation, and PoC writing.

- [Night-Wolf — Reasoning-First Vulnerability Research That Found Multiple Bugs in an Open Source Project](https://blogs.night-wolf.io/reasoning-first-vulnerability-research-that-found-multiples-bugs-in-open-source-project)
  A clean example of a reasoning-first workflow: scope and setup, discovery, triage, private report, coordinated fix. The repeated loop is model, hypothesize, investigate, confirm, record.

- [Night-Wolf — AI-Powered Bug Hunting in Closed Source Software](https://blogs.night-wolf.io/ai-powered-bug-hunting-in-closed-source-software)
  Shows a closed-source workflow around decompilation, skill building, candidate finding, confirmation, and black-box validation evidence.

- [Devansh — Needle in the Haystack: LLMs for Vulnerability Research](https://devansh.bearblog.dev/needle-in-the-haystack/)
  Short field note on why broad prompts create broad hallucinations. The practical takeaway is to build a threat model from prior vulnerability reports and plausible bug classes before asking for findings.

- [Cykor — Building an AI-Based Vulnerability Detection Workflow](https://blog.cykor.kr/2026/06/Building-an-AI-Based-Vulnerability-Detection-Workflow)
  Practical note on the gray zone between code patterns and real vulnerabilities. Useful for designing workflows that distinguish policy decisions from genuinely exploitable behavior.

## Projects

- [Anthropic — Defending Code Reference Harness](https://github.com/anthropics/defending-code-reference-harness)
  Reference harness for agentic vulnerability discovery. Includes a staged workflow, validation-oriented instructions, report generation, and reusable Claude Code skills.

- [Anthropic Reference Harness — Claude Skills](https://github.com/anthropics/defending-code-reference-harness/tree/main/.claude/skills)
  Reusable skills from Anthropic's reference harness. Good examples of encoding security tasks as repeatable agent capabilities.

- [Cloudflare — security-audit-skill](https://github.com/cloudflare/security-audit-skill)
  Coding-agent skill that orchestrates parallel agents through a six-phase pipeline. Notable for adversarial validation — separate agents try to disprove each finding — and cumulative multi-run findings.

- [Visa — Visa Vulnerability Agentic Harness](https://github.com/visa/visa-vulnerability-agentic-harness)
  Visa's open-source agentic SAST harness with threat modeling, multi-lens analysis, adversarial verification, structured findings, remediation, and validation stages.

- [berabuddies — AgentFlow](https://github.com/berabuddies/agentflow)
  Python framework for orchestrating multiple coding agents as dependency graphs with parallel fanout and iterative loops. Kept as the implementation behind the “Synthesizing Multi-Agent Harnesses for Vulnerability Discovery” paper, which reports previously unknown Chrome zero-days rather than a generic web2/pentest workflow.

- [Trail of Bits — Buttercup](https://github.com/trailofbits/buttercup)
  Open-source standalone version of Trail of Bits' second-place AIxCC CRS. Useful for studying a reproducible end-to-end pipeline around AI-augmented input generation, contextual analysis, PoV handling, multi-agent patching, and validation on OSS-Fuzz-style targets.

- [AISLE — nano-analyzer](https://github.com/weareaisle/nano-analyzer)
  Minimal single-file LLM-powered scanner that runs context generation, vulnerability scanning, and skeptical multi-round triage over C/C++ code. Useful as a small, inspectable harness for coverage-first native-code bug hunting.

- [PlamenTSV — plamen](https://github.com/PlamenTSV/plamen)
  Autonomous Web3 security-audit orchestrator for Claude Code and Codex-style workers. It emphasizes mechanical phases, worker artifacts, PoC verification, resumability, and surfaced obligations instead of trusting agent completion claims.

## Papers
- [Synthesizing Multi-Agent Harnesses for Vulnerability Discovery](https://arxiv.org/abs/2604.20801)
  Research on automatically synthesizing multi-agent harnesses via a typed DSL over agent roles, topology, prompts, and tools, reading runtime feedback to propose harness edits. Kept because it reports ten previously unknown zero-days in Google Chrome and has a corresponding implementation in AgentFlow.

- [Knowdit — Agentic Smart Contract Vulnerability Detection with Auditing Knowledge Summarization](https://arxiv.org/html/2603.26270v2)
  Knowledge-driven agentic smart-contract auditing loop: map DeFi semantics and historical vulnerability patterns, generate specifications, synthesize PoCs, execute them, and reflect on findings. Strong fit because it reports confirmed real-world high/medium vulnerabilities and concrete proofs of exploitation.

- [AI Agent Smart Contract Exploit Generation](https://arxiv.org/html/2507.05558v2)
  Describes A1, an agentic exploit-generation system that gathers contract context, generates exploit strategies, tests Solidity PoCs against forked blockchain state, and refines from execution feedback.
