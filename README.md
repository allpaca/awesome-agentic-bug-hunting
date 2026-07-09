# Awesome Agentic Bug Hunting

Languages: English | [한국어](README.ko.md)

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

Each entry is written as a short learning note: what to copy into an agentic bug-hunting system that is trying to find serious, reproducible vulnerabilities rather than generic security observations.

## Articles

- [Anthropic — Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
  - Learn the long-run harness mechanics Anthropic calls out directly: generator/evaluator separation, concrete grading criteria, decomposed chunks, structured artifacts, and context resets with handoffs. For bug hunting, copy this as hunter/verifier separation plus evidence files and reset-safe state so multi-hour scans do not drift into self-approved weak findings.

- [Anthropic — Mythos: Assessing Claude Mythos Preview’s cybersecurity capabilities](https://www.anthropic.com/research/mythos-preview)
  - Learn the Mythos Preview loop from the source: rank files by bug likelihood, send parallel agents at different files, and require bug reports with PoC exploits and reproduction steps. The serious-hunting lesson is to separate coverage and diversity from proof, then let many focused passes generate candidates that still need objective exploit evidence.

- [Anthropic — AI agents find $4.6M in blockchain smart contract exploits](https://www.anthropic.com/research/smart-contracts)
  - Learn how a domain sandbox changes agent behavior: MCP-exposed Foundry/anvil/cast tools, forked-chain state, executable tests, and a hard success signal based on a working exploit. For serious Web3 hunting, the loop should report only concrete, economically meaningful PoCs and treat failed transactions as feedback for the next exploit strategy.

- [OpenAI — Harness Engineering](https://openai.com/index/harness-engineering/)
  - Learn how OpenAI made agent work legible: AGENTS.md as a map, repo-local docs as the system of record, mechanical lint/CI checks, worktree-isolated app instances, DevTools, logs, metrics, and traces. For bug hunting, make the target equally inspectable and executable before autonomy: maps, threat docs, validation commands, observability, and cleanup loops.

- [OpenAI — Codex Security](https://openai.com/index/codex-security-now-in-research-preview/)
  - Learn the product-shaped loop for appsec agents: build system context, create an editable threat model, prioritize by real impact, validate in a sandbox where possible, and propose patches that match system intent. This is useful for filtering serious bugs from checklist noise because severity is grounded in attacker capability, reachability, and project-specific trust boundaries.

- [Google Project Zero — Project Naptime](https://projectzero.google/2024/06/project-naptime.html)
  - Learn agent-interface design for vulnerability research: precise code browsing, reference lookup, Python input generation, debugger access, sanitizer-backed execution, and structured reporting. The key takeaway is to give the model researcher-grade tools with narrow affordances instead of raw unlimited shell, then verify progress through crashes or other objective runtime oracles.

- [Google Project Zero — From Naptime to Big Sleep](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html)
  - Learn how the Naptime idea moves into real-world variant hunting: choose a well-scoped target, adapt failing testcases, reproduce crashes, and write root-cause reports that maintainers can act on. The valuable lesson is the feedback path from hypothesis to testcase to root cause, plus the caution that target-specific fuzzers and human review still matter.

- [Cloudflare — Build Your Own Vulnerability Harness](https://blog.cloudflare.com/build-your-own-vulnerability-harness/)
  - Learn a production-like pipeline shape: recon, hunt, gap fill, trace, dedupe, judgment, fixing, and a validation system that can consume findings from multiple harnesses. The strongest ideas to reuse are staged persistence, micro-forks for narrow investigations, wishlist/resource handoffs, cross-repo tracing, and gates that stop unvalidated leads from becoming reports.

- [Team Atlanta — From Harness to Vulnerability: AI Agents for Code Comprehension and Bug Discovery](https://team-atlanta.github.io/blog/post-mlla-disc-agents/)
  - Learn how AIxCC-scale agents shrink a huge codebase into exploitable leads: identify harness entrypoints, retrieve exact functions, recursively build call graphs, track tainted arguments, and annotate sinks. This is a template for severe bug hunting because exploit generation starts only after the system has proven attacker-controlled data can reach security-relevant operations.

- [Trail of Bits — Buttercup is now open-source!](https://blog.trailofbits.com/2025/08/08/buttercup-is-now-open-source/)
  - Learn the end-to-end vulnerability-discovery architecture behind a top AIxCC system: AI-augmented input generation, contextual static analysis, PoV deduplication, patch generation, and validation. For an autonomous hunter, Buttercup is most useful as a reference for coordinating fuzzing, LLM reasoning, duplicate control, and repair loops without trusting a single agent's conclusion.

- [Xint — Building Effective LLM Agents | AI Cyber Challenge](https://xint.io/blog/building-effective-llm-agents-ai-cyber-challenge-165236)
  - Learn Theori's practical agent-design rules: use agents for tasks humans solve heuristically, decompose vulnerability detection, PoV generation, patching, and root-cause analysis, and constrain each role with curated tools. The source-backed details to copy are structured outputs, a dedicated terminate tool, required tool calls, validation feedback, and progress interventions.

- [Xint — Copy Fail: 732 Bytes to Root on Every Major Linux Distribution](https://xint.io/blog/copy-fail-linux-distributions)
  - Learn how a sharp human invariant plus an agentic search loop can expose a catastrophic kernel bug: page-cache-backed data should not enter writable crypto scatterlists. The reusable pattern is to seed the agent with one precise cross-subsystem suspicion, let it variant-mine the combinatorial surface, then judge severity by the resulting primitive and reproducible exploit path.

- [AISLE — System Over Model: Zero-Day Discovery at the Jagged Frontier](https://aisle.com/blog/system-over-model-zero-day-discovery-at-the-jagged-frontier)
  - Learn why coverage, cheap parallelism, and skeptical triage can beat a single brilliant pass: generate per-file security context, scan function by function, then challenge findings over multiple rounds. This is a strong design for budget-aware serious hunting because it spends tokens seeing more code first, then escalates reasoning only on candidates that survive adversarial review.

- [Microsoft — MDASH](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/)
  - Learn MDASH's concrete stages: prepare language-aware indices and threat models, scan with auditor agents, validate with debaters, dedupe semantic duplicates, and prove bugs with dynamic triggers such as ASan cases. The key lesson is not one agent but 100+ specialized agents whose disagreement, failed refutation, and proof artifacts raise confidence.

- [ClickHouse — How I Hunt for Vulnerabilities with AI](https://clickhouse.com/blog/how-i-hunt-for-vulnerabilities-with-ai)
  - Learn a pragmatic human-in-the-loop workflow for large C++ systems: use LLMs to navigate architecture, propose hypotheses, trace dataflow, and write PoCs, but assume many high-severity-looking findings are false. The lesson for automation is to require local reproduction, containerized setup, exact trigger conditions, and evidence strong enough for a maintainer report.

- [Praetorian — AI Vulnerability Research in the FreeBSD Kernel](https://www.praetorian.com/blog/ai-vulnerability-research-freebsd-kernel/)
  - Learn how to turn kernel auditing into an executable oracle loop: source review, hypothesis, trigger program, instrumented VM, KASAN telemetry, and iterative reproducer repair. For severe native-code hunting, the important piece is the validation environment; the agent should be able to fail, observe sanitizer output, adjust the PoC, and know when to abandon an invalid lead.

- [Security Cryptography Whatever — AI Bug Finding (with Nicholas Carlini)](https://securitycryptographywhatever.com/2026/03/25/ai-bug-finding/)
  - Learn the operational reality of scaling AI bug discovery: reusable harnessing can find many issues quickly, but validation, responsible disclosure, and maintainer-actionable reports become the bottleneck. Use it to design throughput controls: dedupe aggressively, track report quality, budget human review for severe candidates, and measure fix cost as well as find rate.

- [Monad — Monad Bugfinder](https://www.monad.xyz/blog/monad-bugfinder)
  - Learn a Web3 workflow inspired by Mythos and TxRay: separate discovery from validation, persist every lead in a structured database, and treat triage as a distinct job. This is useful because serious autonomous hunting needs a durable source of truth for hypotheses, evidence, duplicates, exploit attempts, and reviewer decisions rather than chat transcript memory.

- [Gustavo Grieco — Introducing Quimera](https://gustavo-grieco.github.io/blog/introducing-quimera/)
  - Learn feedback-driven exploit generation for Ethereum contracts: combine source code, on-chain context, Foundry traces, and Solidity PoC iteration around a single high-impact goal, draining funds. The lesson is to narrow the vulnerability class, wire in an executable success signal, and let trace failures guide the next strategy instead of asking for broad audit commentary.

- [Yue Xue — AI Auditing Methodology: Agent Self-Evolution, Drift, Reverse Evolution and Solutions](https://www.linkedin.com/pulse/ai-auditing-methodology-agent-self-evolution-drift-reverse-yue-xue-uqsse/)
  - Learn how long-running audit systems fail: vulnerability drift moves the agent to easier nearby issues, and reverse evolution makes prompts bigger without improving hit rate. The reusable design is decomposition → task definition → reasoning, with each task anchored by target, boundary, witness, completion condition, verifier, and local evolution rules.

- [Yue Xue — AI Auditing Methodology, Part I](https://x.com/xy9301/status/2033186266649640980)
  - Learn the X Article's core thesis: AI security is shifting from framework-driven pipelines to coding-agent-driven workflows that first reproduce the human audit process and then automate it. For a serious hunter, start with flexible agent exploration over rich project material instead of over-preprocessing everything into a brittle static framework.

- [Yue Xue — AI Auditing Methodology, Part II](https://x.com/xy9301/status/2036017855381340269)
  - Learn the self-evolution requirements named in the article: variation, selection, and inheritance. For bug-hunting systems, that means trying new prompts/skills/decompositions, judging them with benchmarks or human review, retaining rollbackable experience, and treating dedupe, validation, and PoC generation as workflow-critical—not demo polish.

- [pkqs91 / Octane Security — How I Made $200k With Codex in 3 Months](https://x.com/pkqs91/status/2070157806104457395)
  - Learn the actual bug-bounty loop described in the X Article: intake turns scope, severity rules, exclusions, docs, and code into a local bundle; hunting explores wide; verification kills out-of-scope, guarded, or no-impact leads. The durable rule is “explore wide, exploit deep” and manually reproduce only the few candidates that survive impact filtering.

- [Plamen Tsanev — BEAST post](https://x.com/p_tsanev/status/2041880807032119670)
  - Learn why the first “few agents over the whole codebase” experiment failed, and why Plamen moved to specialized recursion. The article lays out an 8-phase flow—recon, instantiation, breadth, inventory/semantic analysis, depth, chain analysis, PoC, and report—useful for finding compound logic bugs that fatigue-prone humans miss.

- [Zero Cool Labs — Symmetry Sniper](https://x.com/ZeroCool_AI/status/2069443859327705497)
  - Learn the narrow-skill pattern from the X Article: establish an intended mirror from docs, names, events, interfaces, or tests, then compare writes, asset movement, authorization, rounding, zero cases, and bounds. It reports only measurable impacts such as invariant breaks, auth mismatches, fee/rounding asymmetry, or batch bypasses—good discipline for high-signal skills.

- [Alin-Mihai BARBATEI — Notes on building a smart contract security auditing harness](https://www.linkedin.com/posts/alin-mihai-barbatei-27772b54_notes-on-building-a-smart-contract-security-activity-7476636721527455744-favn)
  - Learn the lifecycle taxonomy for a Web3 audit harness: discovery/recon, lead generation, finding identification, issue validation, and PoC writing. This is valuable as a checklist for assigning agents to stages and for ensuring every candidate has moved from "interesting lead" to "validated exploit scenario" before it reaches a report.

- [Night-Wolf — Reasoning-First Vulnerability Research That Found Multiple Bugs in an Open Source Project](https://blogs.night-wolf.io/reasoning-first-vulnerability-research-that-found-multiples-bugs-in-open-source-project)
  - Learn the article's explicit loop: MODEL the target, HYPOTHESIZE attacker-controlled input and violated assumption, INVESTIGATE dataflow by reading code, CONFIRM with a minimal local PoC, and RECORD or discard. This is a clean template for forcing agents to cross from reasoning into evidence before private disclosure.

- [Night-Wolf — AI-Powered Bug Hunting in Closed Source Software](https://blogs.night-wolf.io/ai-powered-bug-hunting-in-closed-source-software)
  - Learn how to adapt agentic hunting when source is unavailable: decompile, build a decompiler skill, run multi-angle review, filter impossible preconditions, and validate candidates black-box. The severe-bug lesson is to treat reversing artifacts as lossy evidence and require runtime confirmation that the claimed attacker path exists in the real binary.

- [Devansh — Needle in the Haystack: LLMs for Vulnerability Research](https://devansh.bearblog.dev/needle-in-the-haystack/)
  - Learn why vague "find bugs" prompts fail: without threat model, attacker capability, trust boundaries, and prior CVE context, models produce broad CWE-shaped hallucinations. The reusable scaffold is intentionally small: pick a thin slice, build a one-page threat model, identify invariants and entrypoints, then ask for call chains and tests that prove exploitability.

- [Cykor — Building an AI-Based Vulnerability Detection Workflow](https://blog.cykor.kr/2026/06/Building-an-AI-Based-Vulnerability-Detection-Workflow)
  - Learn the three-stage workflow that emerged from practice: preprocess the target, generate vulnerability hypotheses, and validate candidates with separated responsibilities. The subtle lesson is policy context: many findings live in a gray zone, so the agent must know service intent and responsibility boundaries before calling a code pattern exploitable.

## Projects

- [Anthropic — Defending Code Reference Harness](https://github.com/anthropics/defending-code-reference-harness)
  - Learn a concrete reference implementation of recon → find → verify → report → patch for C/C++ memory bugs with Docker, ASAN, gVisor isolation, and parallel agents. The reusable parts are the stage contracts, sandbox refusal rules, fresh-container verification, dedupe, exploitability reporting, and patch validation against both the original PoC and renewed search.

- [Anthropic Reference Harness — Claude Skills](https://github.com/anthropics/defending-code-reference-harness/tree/main/.claude/skills)
  - Learn how to encode security work as reusable agent skills: quickstart, threat-model, vuln-scan, triage, patch, and customize. These are useful templates for turning expert review habits into repeatable commands with inputs, artifacts, safety boundaries, and expected outputs rather than relying on ad hoc chat instructions.

- [Cloudflare — security-audit-skill](https://github.com/cloudflare/security-audit-skill)
  - Learn a compact six-phase coding-agent audit skill: parallel recon, multi-angle hunt, adversarial validation, dedupe, cumulative findings, and machine-readable outputs. The most important reusable rule is that the validator must be separate from the finder and should try to disprove exploitability before a finding is allowed to survive.

- [Visa — Visa Vulnerability Agentic Harness](https://github.com/visa/visa-vulnerability-agentic-harness)
  - Learn a vendor-neutral SAST harness with threat modeling, multi-lens research, deterministic voting, adversarial verification, structured Markdown/SARIF findings, remediation, and validation. It is especially useful for enterprise systems because it optimizes the full lifecycle from AI-discovered exploitability to a validated fix, not just candidate generation.

- [berabuddies — AgentFlow](https://github.com/berabuddies/agentflow)
  - Learn how to express agentic bug-hunting systems as typed graphs: fanout over files or hypotheses, merge findings, loop on reviewer failure, run mixed models, and evolve agents from traces. This is the implementation pattern to study when a vulnerability workflow needs thousands of bounded agents, dependency edges, shared scratchboards, and measurable stop conditions.

- [Trail of Bits — Buttercup](https://github.com/trailofbits/buttercup)
  - Learn a runnable AIxCC-derived vulnerability-finding and patching pipeline for OSS-Fuzz-like targets with task monitoring, fuzzing, PoV handling, and patch generation. Use it to study how traditional dynamic testing and LLM agents can share state, avoid duplicate work, and produce fixes that are validated rather than merely suggested.

- [AISLE — nano-analyzer](https://github.com/weareaisle/nano-analyzer)
  - Learn the smallest useful version of an LLM-powered native-code scanner: per-file context generation, vulnerability scan, skeptical triage, and final arbitration. It is a good baseline for severe-bug systems because every extra agentic feature can be compared against a cheap coverage-first pipeline that already finds real candidates.

- [PlamenTSV — plamen](https://github.com/PlamenTSV/plamen)
  - Learn the runnable version of the BEAST idea: 18–100 Claude/Codex agents across 8 phases producing audit reports with verified PoC exploits for smart contracts and L1 node-client infrastructure. The useful engineering details are PTY-supervised workers, disk artifacts as truth, resumable checkpoints, PoC pass/fail markers, and surfaced obligations.

## Papers

- [Synthesizing Multi-Agent Harnesses for Vulnerability Discovery](https://arxiv.org/abs/2604.20801)
  - Learn how to optimize the harness itself, not just the prompts: roles, tools, communication topology, retries, and coordination protocol are modeled in a typed graph DSL. The key lesson is that runtime feedback from the target can diagnose which harness component failed, enabling systematic rewrites that reportedly found previously unknown Chrome zero-days.

- [Knowdit — Agentic Smart Contract Vulnerability Detection with Auditing Knowledge Summarization](https://arxiv.org/html/2603.26270v2)
  - Learn knowledge-driven smart-contract auditing from the paper: build an auditing knowledge graph from human reports, map DeFi semantics and bug patterns onto a target, then iterate specification generation, PoC synthesis, execution, and reflection. Its reported 9 high- and 36 medium-severity unknown findings show why domain knowledge plus executable proof beats generic Solidity smell scanning.

- [AI Agent Smart Contract Exploit Generation](https://arxiv.org/html/2507.05558v2)
  - Learn A1's execution-driven exploit model: give the LLM domain tools, generate strategies, run Solidity PoCs against forked chain state, and refine from execution feedback. The paper's key discipline is reporting only concretely validated profitable PoC exploits, with a 62.96% VERITE success rate—useful as a hard standard for Web3 agent outputs.
