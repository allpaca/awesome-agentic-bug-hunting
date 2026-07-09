# Awesome Agentic Bug Hunting

Languages: English | [한국어](README.ko.md)

A curated list of public resources about using coding agents, LLM-driven harnesses, skills, and repeatable workflows to find and validate real software vulnerabilities.

The focus is practical agentic bug hunting: source-code or binary exploration, vulnerability hypothesis generation, validation, PoC construction, triage, and repeatable harness design.

Last reviewed: 2026-07-09

## Contents

- [About this list](#about-this-list)
- [Articles](#articles)
- [Papers](#papers)
- [Projects](#projects)
- [Reference](#reference)

## About this list

**What to include.** A resource should show at least one of the following:

- A concrete LLM- or agent-driven vulnerability discovery workflow.
- A reusable harness, scaffold, agent skill, agent architecture, or validation loop.
- A case study where an agent helped find, validate, reproduce, or report real vulnerabilities.
- A research method where the LLM directly participates in discovery, hypothesis generation, validation, or specification inference.

Each entry is written as a short learning note: what to copy into an agentic bug-hunting system that is trying to find serious, reproducible vulnerabilities rather than generic security observations.

## Articles
- [2026-06-30 — Yue Xue — AI Auditing Methodology: Agent Self-Evolution, Drift, Reverse Evolution and Solutions](https://www.linkedin.com/pulse/ai-auditing-methodology-agent-self-evolution-drift-reverse-yue-xue-uqsse/)
  - Learn how long-running audit systems fail: vulnerability drift moves the agent to easier nearby issues, and reverse evolution makes prompts bigger without improving hit rate. The reusable design is decomposition → task definition → reasoning, with each task anchored by target, boundary, witness, completion condition, verifier, and local evolution rules.

- [2026-06-26 — ClickHouse — How I Hunt for Vulnerabilities with AI](https://clickhouse.com/blog/how-i-hunt-for-vulnerabilities-with-ai)
  - Learn a pragmatic human-in-the-loop workflow for large C++ systems: use LLMs to navigate architecture, propose hypotheses, trace dataflow, and write PoCs, but assume many high-severity-looking findings are false. The lesson for automation is to require local reproduction, containerized setup, exact trigger conditions, and evidence strong enough for a maintainer report.

- [2026-06-25 — pkqs91 / Octane Security — How I Made $200k With Codex in 3 Months](https://x.com/pkqs91/status/2070157806104457395)
  - Learn the actual bug-bounty loop described in the X Article: intake turns scope, severity rules, exclusions, docs, and code into a local bundle; hunting explores wide; verification kills out-of-scope, guarded, or no-impact leads. The durable rule is “explore wide, exploit deep” and manually reproduce only the few candidates that survive impact filtering.

- [2026-06-23 — Zero Cool Labs — Symmetry Sniper](https://x.com/ZeroCool_AI/status/2069443859327705497)
  - Learn the narrow-skill pattern from the X Article: establish an intended mirror from docs, names, events, interfaces, or tests, then compare writes, asset movement, authorization, rounding, zero cases, and bounds. It reports only measurable impacts such as invariant breaks, auth mismatches, fee/rounding asymmetry, or batch bypasses—good discipline for high-signal skills.

- [2026-06-18 — Cloudflare — Build Your Own Vulnerability Harness](https://blog.cloudflare.com/build-your-own-vulnerability-harness/)
  - Learn a production-like pipeline shape: recon, hunt, gap fill, trace, dedupe, judgment, fixing, and a validation system that can consume findings from multiple harnesses. The strongest ideas to reuse are staged persistence, micro-forks for narrow investigations, wishlist/resource handoffs, cross-repo tracing, and gates that stop unvalidated leads from becoming reports.

- [2026-06-17 — Praetorian — AI Vulnerability Research in the FreeBSD Kernel](https://www.praetorian.com/blog/ai-vulnerability-research-freebsd-kernel/)
  - Learn how to turn kernel auditing into an executable oracle loop: source review, hypothesis, trigger program, instrumented VM, KASAN telemetry, and iterative reproducer repair. For severe native-code hunting, the important piece is the validation environment; the agent should be able to fail, observe sanitizer output, adjust the PoC, and know when to abandon an invalid lead.

- [2026-06-01 — Night-Wolf — AI-Powered Bug Hunting in Closed Source Software](https://blogs.night-wolf.io/ai-powered-bug-hunting-in-closed-source-software)
  - Learn how to adapt agentic hunting when source is unavailable: decompile, build a decompiler skill, run multi-angle review, filter impossible preconditions, and validate candidates black-box. The severe-bug lesson is to treat reversing artifacts as lossy evidence and require runtime confirmation that the claimed attacker path exists in the real binary.

- [2026-06-01 — Cykor — Building an AI-Based Vulnerability Detection Workflow](https://blog.cykor.kr/2026/06/Building-an-AI-Based-Vulnerability-Detection-Workflow)
  - Learn the three-stage workflow that emerged from practice: preprocess the target, generate vulnerability hypotheses, and validate candidates with separated responsibilities. The subtle lesson is policy context: many findings live in a gray zone, so the agent must know service intent and responsibility boundaries before calling a code pattern exploitable.

- [2026-05-27 — Night-Wolf — Reasoning-First Vulnerability Research That Found Multiple Bugs in an Open Source Project](https://blogs.night-wolf.io/reasoning-first-vulnerability-research-that-found-multiples-bugs-in-open-source-project)
  - Learn the article's explicit loop: MODEL the target, HYPOTHESIZE attacker-controlled input and violated assumption, INVESTIGATE dataflow by reading code, CONFIRM with a minimal local PoC, and RECORD or discard. This is a clean template for forcing agents to cross from reasoning into evidence before private disclosure.

- [2026-05-24 — Monad — Monad Bugfinder](https://www.monad.xyz/blog/monad-bugfinder)
  - Learn a Web3 workflow inspired by Mythos and TxRay: separate discovery from validation, persist every lead in a structured database, and treat triage as a distinct job. This is useful because serious autonomous hunting needs a durable source of truth for hypotheses, evidence, duplicates, exploit attempts, and reviewer decisions rather than chat transcript memory.

- [2026-05-12 — Microsoft — MDASH](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/)
  - Learn MDASH's concrete stages: prepare language-aware indices and threat models, scan with auditor agents, validate with debaters, dedupe semantic duplicates, and prove bugs with dynamic triggers such as ASan cases. The key lesson is not one agent but 100+ specialized agents whose disagreement, failed refutation, and proof artifacts raise confidence.

- [2026-04-29 — Xint — Copy Fail: 732 Bytes to Root on Every Major Linux Distribution](https://xint.io/blog/copy-fail-linux-distributions)
  - Learn how a sharp human invariant plus an agentic search loop can expose a catastrophic kernel bug: page-cache-backed data should not enter writable crypto scatterlists. The reusable pattern is to seed the agent with one precise cross-subsystem suspicion, let it variant-mine the combinatorial surface, then judge severity by the resulting primitive and reproducible exploit path.

- [2026-04-14 — AISLE — System Over Model: Zero-Day Discovery at the Jagged Frontier](https://aisle.com/blog/system-over-model-zero-day-discovery-at-the-jagged-frontier)
  - Learn why coverage, cheap parallelism, and skeptical triage can beat a single brilliant pass: generate per-file security context, scan function by function, then challenge findings over multiple rounds. This is a strong design for budget-aware serious hunting because it spends tokens seeing more code first, then escalates reasoning only on candidates that survive adversarial review.

- [2026-04-08 — Plamen Tsanev — BEAST post](https://x.com/p_tsanev/status/2041880807032119670)
  - Learn why the first “few agents over the whole codebase” experiment failed, and why Plamen moved to specialized recursion. The article lays out an 8-phase flow—recon, instantiation, breadth, inventory/semantic analysis, depth, chain analysis, PoC, and report—useful for finding compound logic bugs that fatigue-prone humans miss.

- [2026-04-07 — Anthropic — Mythos: Assessing Claude Mythos Preview’s cybersecurity capabilities](https://www.anthropic.com/research/mythos-preview)
  - Learn the Mythos Preview loop from the source: rank files by bug likelihood, send parallel agents at different files, and require bug reports with PoC exploits and reproduction steps. The serious-hunting lesson is to separate coverage and diversity from proof, then let many focused passes generate candidates that still need objective exploit evidence.

- [2026-03-25 — Security Cryptography Whatever — AI Bug Finding (with Nicholas Carlini)](https://securitycryptographywhatever.com/2026/03/25/ai-bug-finding/)
  - Learn the operational reality of scaling AI bug discovery: reusable harnessing can find many issues quickly, but validation, responsible disclosure, and maintainer-actionable reports become the bottleneck. Use it to design throughput controls: dedupe aggressively, track report quality, budget human review for severe candidates, and measure fix cost as well as find rate.

- [2026-03-24 — Anthropic — Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
  - Learn the long-run harness mechanics Anthropic calls out directly: generator/evaluator separation, concrete grading criteria, decomposed chunks, structured artifacts, and context resets with handoffs. For bug hunting, copy this as hunter/verifier separation plus evidence files and reset-safe state so multi-hour scans do not drift into self-approved weak findings.

- [2026-03-23 — Yue Xue — AI Auditing Methodology, Part II](https://x.com/xy9301/status/2036017855381340269)
  - Learn the self-evolution requirements named in the article: variation, selection, and inheritance. For bug-hunting systems, that means trying new prompts/skills/decompositions, judging them with benchmarks or human review, retaining rollbackable experience, and treating dedupe, validation, and PoC generation as workflow-critical—not demo polish.

- [2026-03-16 — OpenAI — Why Codex Security Doesn't Include a SAST Report](https://openai.com/index/why-codex-security-doesnt-include-sast/)
  - Learn the concrete design stance behind Codex Security: do not start from generic source-to-sink alerts; start from behavior, constraints, reachable exploit paths, and sandbox validation. For agentic bug hunting, reuse this as a filter: a finding needs attacker-controlled input, failed defense assumptions, executable proof when possible, and a patch that preserves intended behavior.

- [2026-03-15 — Yue Xue — AI Auditing Methodology, Part I](https://x.com/xy9301/status/2033186266649640980)
  - Learn the X Article's core thesis: AI security is shifting from framework-driven pipelines to coding-agent-driven workflows that first reproduce the human audit process and then automate it. For a serious hunter, start with flexible agent exploration over rich project material instead of over-preprocessing everything into a brittle static framework.

- [2026-03-09 — Devansh — Needle in the Haystack: LLMs for Vulnerability Research](https://devansh.bearblog.dev/needle-in-the-haystack/)
  - Learn why vague "find bugs" prompts fail: without threat model, attacker capability, trust boundaries, and prior CVE context, models produce broad CWE-shaped hallucinations. The reusable scaffold is intentionally small: pick a thin slice, build a one-page threat model, identify invariants and entrypoints, then ask for call chains and tests that prove exploitability.

- [2026-02-11 — OpenAI — Harness Engineering](https://openai.com/index/harness-engineering/)
  - Learn how OpenAI made agent work legible: AGENTS.md as a map, repo-local docs as the system of record, mechanical lint/CI checks, worktree-isolated app instances, DevTools, logs, metrics, and traces. For bug hunting, make the target equally inspectable and executable before autonomy: maps, threat docs, validation commands, observability, and cleanup loops.

- [2025-12-01 — Anthropic — AI agents find $4.6M in blockchain smart contract exploits](https://www.anthropic.com/research/smart-contracts)
  - Learn how a domain sandbox changes agent behavior: MCP-exposed Foundry/anvil/cast tools, forked-chain state, executable tests, and a hard success signal based on a working exploit. For serious Web3 hunting, the loop should report only concrete, economically meaningful PoCs and treat failed transactions as feedback for the next exploit strategy.

- [2025-09-04 — Team Atlanta — From Harness to Vulnerability: AI Agents for Code Comprehension and Bug Discovery](https://team-atlanta.github.io/blog/post-mlla-disc-agents/)
  - Learn how AIxCC-scale agents shrink a huge codebase into exploitable leads: identify harness entrypoints, retrieve exact functions, recursively build call graphs, track tainted arguments, and annotate sinks. This is a template for severe bug hunting because exploit generation starts only after the system has proven attacker-controlled data can reach security-relevant operations.

- [2025-08-08 — Trail of Bits — Buttercup is now open-source!](https://blog.trailofbits.com/2025/08/08/buttercup-is-now-open-source/)
  - Learn the end-to-end vulnerability-discovery architecture behind a top AIxCC system: AI-augmented input generation, contextual static analysis, PoV deduplication, patch generation, and validation. For an autonomous hunter, Buttercup is most useful as a reference for coordinating fuzzing, LLM reasoning, duplicate control, and repair loops without trusting a single agent's conclusion.

- [2025-08-08 — Xint — Building Effective LLM Agents | AI Cyber Challenge](https://xint.io/blog/building-effective-llm-agents-ai-cyber-challenge-165236)
  - Learn Theori's practical agent-design rules: use agents for tasks humans solve heuristically, decompose vulnerability detection, PoV generation, patching, and root-cause analysis, and constrain each role with curated tools. The source-backed details to copy are structured outputs, a dedicated terminate tool, required tool calls, validation feedback, and progress interventions.

- [2025-07-03 — Gustavo Grieco — Introducing Quimera](https://gustavo-grieco.github.io/blog/introducing-quimera/)
  - Learn feedback-driven exploit generation for Ethereum contracts: combine source code, on-chain context, Foundry traces, and Solidity PoC iteration around a single high-impact goal, draining funds. The lesson is to narrow the vulnerability class, wire in an executable success signal, and let trace failures guide the next strategy instead of asking for broad audit commentary.

- [2024-11-01 — Google Project Zero — From Naptime to Big Sleep](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html)
  - Learn the first public Big Sleep case study: LLM-assisted variant hunting over real code, adapting failing testcases, reproducing crashes, and writing maintainer-actionable root-cause reports. Use it for the concrete feedback loop from hypothesis to testcase to root cause, while treating it as an early case study rather than the current state of Big Sleep operations.

## Papers
- [2026-07-08 — Thinking More, Harnessing Better: State Machine Guided Harness Automatic Generation with Project Digestion and Workflow Decomposition](https://arxiv.org/abs/2607.07007)
  - Learn SynapseFlow's staged harness-generation workflow even if your target is not fuzzing: digest the project into structural flow graphs, aggregate coherent function triplets, then synthesize harnesses through a four-stage process with rollback when correctness breaks. The transferable lesson is to make harness construction decomposed, measurable, and failure-recoverable instead of trusting one-shot generation.

- [2026-07 — Thought Is All You Need: Smart Contract Vulnerability Detection with Thought-Augmented Large Language Model](https://yajin.org/papers/fse2026_synapse.pdf)
  - Learn Synapse's thought-augmented smart-contract auditing loop: distill reusable vulnerability-reasoning thoughts from audit reports, instantiate them against focal code context, then coordinate Developer, Researcher, Auditor, and Verifier agents with semantic tools. It is especially useful for logic/state bugs because the workflow targets missing state updates and contract semantics that fixed-pattern scanners and non-semantic fuzzing oracles tend to miss.

- [2026-07-01 — Knowdit — Agentic Smart Contract Vulnerability Detection with Auditing Knowledge Summarization](https://arxiv.org/html/2603.26270v2)
  - Learn knowledge-driven smart-contract auditing from the paper: build an auditing knowledge graph from human reports, map DeFi semantics and bug patterns onto a target, then iterate specification generation, PoC synthesis, execution, and reflection. Its reported 9 high- and 36 medium-severity unknown findings show why domain knowledge plus executable proof beats generic Solidity smell scanning.

- [2026-06-20 — Revelio: Cost-Efficient Agentic Memory Safety Vulnerability Detection for Repository-Scale Codebases](https://arxiv.org/abs/2606.22263)
  - Learn a hallucination-resistant validation gate for repository-scale agents: inexpensive LLMs and lightweight static analysis generate and rank hypotheses, but a finding survives only when an executable Proof-of-Vulnerability is reproduced under a deterministic sanitizer. It is memory-safety focused rather than logic-bug focused, yet the generate-rank-prove contract is a strong pattern for any serious bug-hunting system.

- [2026-05-12 — VulTriage: Triple-Path Context Augmentation for LLM-Based Vulnerability Detection](https://arxiv.org/abs/2605.09461)
  - Learn a context-packing stage for subtle semantic vulnerabilities: a Control Path verbalizes AST, CFG, and DFG facts, a Knowledge Path retrieves CWE-derived patterns and examples, and a Semantic Path summarizes behavior before the final judgment. It is not a full autonomous agent, but it is a useful harness component for giving reviewers structured control-flow, data-flow, prior-knowledge, and behavior context.

- [2026-04-22 — Synthesizing Multi-Agent Harnesses for Vulnerability Discovery](https://arxiv.org/abs/2604.20801)
  - Learn how to optimize the harness itself, not just the prompts: roles, tools, communication topology, retries, and coordination protocol are modeled in a typed graph DSL. The key lesson is that runtime feedback from the target can diagnose which harness component failed, enabling systematic rewrites that reportedly found previously unknown Chrome zero-days.

- [2026-03-28 — VulInstruct: Teaching LLMs Root-Cause Reasoning for Vulnerability Detection via Security Specifications](https://arxiv.org/abs/2511.04014)
  - Learn specification-guided root-cause reasoning: extract safe-behavior expectations from historical patches and repository-specific repeated violations, retrieve relevant specs and cases, then force the model to judge code behavior against the expected security property. This is useful for logical-bug hunting because it moves the agent away from CWE-pattern matching and toward invariant/specification mismatch with explicit reasoning.

- [2026-02-10 — QRS: A Rule-Synthesizing Neuro-Symbolic Triad for Autonomous Vulnerability Discovery](https://arxiv.org/abs/2602.09774)
  - Learn the Query, Review, Sanitize harness shape: autonomous agents synthesize CodeQL queries from schemas and examples, semantically review findings, and push candidates toward automated exploit synthesis. Although CodeQL is in the loop, the transferable idea is not canned SAST; it is agent-generated checks plus independent semantic and exploit-oriented validation.

- [2026-02-03 — LogicScan: An LLM-driven Framework for Detecting Business Logic Vulnerabilities in Smart Contracts](https://arxiv.org/abs/2602.03271)
  - Learn a business-logic bug workflow for smart contracts: mine invariants from mature deployed protocols, normalize them into a Business Specification Language, and contrast target contracts against those reference constraints. This is directly aligned with logical bugs because the target is missing or weakly enforced business invariants, not reentrancy, arithmetic, or other fixed signatures.

- [2026-01-27 — AgenticSCR: An Autonomous Agentic Secure Code Review for Immature Vulnerabilities Detection](https://arxiv.org/abs/2601.19138)
  - Learn a pre-commit secure-review agent design: autonomous code navigation, tool invocation, and security-focused semantic memories for context-dependent immature vulnerabilities. The reusable lesson is to make review comments localize, explain, and challenge risky changes before they become production bugs, instead of relying on noisy SAST output after the fact.

- [2026-01-12 — AI Agent Smart Contract Exploit Generation](https://arxiv.org/html/2507.05558v2)
  - Learn A1's execution-driven exploit model: give the LLM domain tools, generate strategies, run Solidity PoCs against forked chain state, and refine from execution feedback. The paper's key discipline is reporting only concretely validated profitable PoC exploits, with a 62.96% VERITE success rate—useful as a hard standard for Web3 agent outputs.

- [2025-12-08 — VulnLLM-R: Specialized Reasoning LLM with Agent Scaffold for Vulnerability Detection](https://arxiv.org/abs/2512.07533)
  - Learn how a vulnerability-reasoning model is wrapped in an agent scaffold for project-level hunting: reason about program states, apply test-time optimization, and validate on real repositories against CodeQL and AFL++ baselines. It is less logic-bug-specific than LogicScan or Synapse, but useful for separating model specialization, agent scaffolding, and real-world finding validation.

- [2025-08 — LLMxCPG: Context-Aware Vulnerability Detection Through Code Property Graph-Guided Large Language Models](https://www.usenix.org/conference/usenixsecurity25/presentation/lekssays)
  - Learn a context reducer for repository-scale agents: construct CPG-based slices that shrink code while preserving vulnerability-relevant context across function-level and multi-function cases. This is not an agentic workflow by itself, but it is a practical pre-agent layer for deciding what code, dependencies, and syntactic variants an LLM reviewer should see.

- [2025-07 — Benchmarking LLMs and LLM-based Agents in Practical Vulnerability Detection for Code Repositories](https://aclanthology.org/2025.acl-long.1490/)
  - Learn an evaluation harness for repository-level agents: JITVul links vulnerable functions to vulnerability-introducing and fixing commits, forcing agents to handle interprocedural, multi-hop context rather than isolated snippets. Its ReAct-agent results are useful as a reality check for bug-hunting harnesses because better context and tool use still produce over-analysis and guard misreadings.

- [2024-09-14 — Combining Fine-Tuning and LLM-based Agents for Intuitive Smart Contract Auditing with Justifications](https://arxiv.org/abs/2403.16073)
  - Learn iAudit's two-stage smart-contract audit loop: a Detector identifies likely vulnerable code, a Reasoner proposes causes, then Ranker and Critic agents debate which cause best explains the evidence. The useful workflow lesson is that logical smart-contract findings need cause selection and adversarial justification, not just a binary vulnerable/not-vulnerable label.

## Projects
- [Cloudflare — security-audit-skill](https://github.com/cloudflare/security-audit-skill)
  - Learn a compact six-phase coding-agent audit skill: parallel recon, multi-angle hunt, adversarial validation, dedupe, cumulative findings, and machine-readable outputs. The most important reusable rule is that the validator must be separate from the finder and should try to disprove exploitability before a finding is allowed to survive.

- [Visa — Visa Vulnerability Agentic Harness](https://github.com/visa/visa-vulnerability-agentic-harness)
  - Learn a vendor-neutral SAST harness with threat modeling, multi-lens research, deterministic voting, adversarial verification, structured Markdown/SARIF findings, remediation, and validation. It is especially useful for enterprise systems because it optimizes the full lifecycle from AI-discovered exploitability to a validated fix, not just candidate generation.

- [Anthropic — Defending Code Reference Harness](https://github.com/anthropics/defending-code-reference-harness)
  - Learn a concrete reference implementation of recon → find → verify → report → patch for C/C++ memory bugs with Docker, ASAN, gVisor isolation, and parallel agents. The reusable parts are the stage contracts, sandbox refusal rules, fresh-container verification, dedupe, exploitability reporting, and patch validation against both the original PoC and renewed search.

- [Anthropic Reference Harness — Claude Skills](https://github.com/anthropics/defending-code-reference-harness/tree/main/.claude/skills)
  - Learn how to encode security work as reusable agent skills: quickstart, threat-model, vuln-scan, triage, patch, and customize. These are useful templates for turning expert review habits into repeatable commands with inputs, artifacts, safety boundaries, and expected outputs rather than relying on ad hoc chat instructions.

- [AISLE — nano-analyzer](https://github.com/weareaisle/nano-analyzer)
  - Learn the smallest useful version of an LLM-powered native-code scanner: per-file context generation, vulnerability scan, skeptical triage, and final arbitration. It is a good baseline for severe-bug systems because every extra agentic feature can be compared against a cheap coverage-first pipeline that already finds real candidates.

- [PlamenTSV — plamen](https://github.com/PlamenTSV/plamen)
  - Learn the runnable version of the BEAST idea: 18–100 Claude/Codex agents across 8 phases producing audit reports with verified PoC exploits for smart contracts and L1 node-client infrastructure. The useful engineering details are PTY-supervised workers, disk artifacts as truth, resumable checkpoints, PoC pass/fail markers, and surfaced obligations.

- [berabuddies — AgentFlow](https://github.com/berabuddies/agentflow)
  - Learn how to express agentic bug-hunting systems as typed graphs: fanout over files or hypotheses, merge findings, loop on reviewer failure, run mixed models, and evolve agents from traces. This is the implementation pattern to study when a vulnerability workflow needs thousands of bounded agents, dependency edges, shared scratchboards, and measurable stop conditions.

- [Trail of Bits — Buttercup](https://github.com/trailofbits/buttercup)
  - Learn a runnable AIxCC-derived vulnerability-finding and patching pipeline for OSS-Fuzz-like targets with task monitoring, fuzzing, PoV handling, and patch generation. Use it to study how traditional dynamic testing and LLM agents can share state, avoid duplicate work, and produce fixes that are validated rather than merely suggested.

## Reference
- [Alin-Mihai BARBATEI — Notes on building a smart contract security auditing harness](https://www.linkedin.com/posts/alin-mihai-barbatei-27772b54_notes-on-building-a-smart-contract-security-activity-7476636721527455744-favn)
  - Referenced while selecting smart-contract auditing Articles and workflow-oriented resources. It helped frame the recurring harness stages used in this list: codebase context/recon, lead generation, finding identification, issue validation, and PoC writing.

- [huhusmang — Awesome LLMs for Vulnerability Detection](https://github.com/huhusmang/Awesome-LLMs-for-Vulnerability-Detection)
  - Referenced while expanding the Papers section. It served as a broad index for LLM-based vulnerability-detection papers; this list only pulled in the entries that were most relevant to agentic bug hunting, workflow design, harness construction, semantic reasoning, or exploit validation.
