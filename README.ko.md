# Awesome Agentic Bug Hunting 한국어판

언어: [English](README.md) | 한국어

코딩 에이전트, LLM 기반 하네스, 보안 스킬, 반복 가능한 워크플로를 활용해 실제 소프트웨어 취약점을 찾고 검증하는 공개 자료 모음입니다.

이 리스트는 실전형 에이전트 기반 버그 헌팅에 초점을 둡니다. 소스코드나 바이너리 탐색, 취약점 가설 생성, 검증, PoC 작성, 분류/검토, 재사용 가능한 하네스 설계를 다룹니다.

마지막 검토일: 2026-07-10

## 목차

- [이 리스트에 대하여](#이-리스트에-대하여)
- [아티클](#아티클)
- [논문](#논문)
- [프로젝트](#프로젝트)
- [레퍼런스](#레퍼런스)

## 이 리스트에 대하여

**포함 기준.** 자료는 agentic bug-hunting system에 필요한 실용적인 구성요소를 하나 이상 제공해야 합니다.

- **발견 워크플로:** LLM 또는 에이전트가 대상을 탐색하고, 취약점 가설을 만들고, 공격자가 제어하는 경로를 추론하거나, 누락된 명세를 유추하는 방법.
- **하네스와 검증 설계:** 재사용 가능한 scaffold, 에이전트 역할, 스킬, 도구 루프, 상태 관리, coverage tracking, PoC 실행, verifier/critic gate.
- **실제 취약점 증거:** LLM 또는 에이전트가 실제 취약점을 찾고, 검증하고, 재현하고, exploit하고, 보고하거나, 패치하는 데 도움을 준 사례.
- **논리 버그 방법론:** 깨진 불변식, 위험한 상태 전이, 신뢰 경계 오류, 프로토콜 또는 컨트랙트 동작 불일치, 고정 시그니처만으로 잡기 어려운 공격 경로를 찾는 방법.

각 항목은 단순 요약이 아니라, “AI agent로 심각하고 재현 가능한 논리 버그를 자동으로 찾는 구조에 무엇을 가져올 수 있는가”를 짧게 정리한 학습 노트입니다.

## 아티클
- [2026-06-30 — Yue Xue — AI Auditing Methodology: Agent Self-Evolution, Drift, Reverse Evolution and Solutions](https://www.linkedin.com/pulse/ai-auditing-methodology-agent-self-evolution-drift-reverse-yue-xue-uqsse/)
  - 장기 실행 감사 시스템이 어떻게 실패하는지 배울 수 있습니다. vulnerability drift는 에이전트를 더 쉬운 주변 문제로 끌고 가고, reverse evolution은 프롬프트만 길게 만들 뿐 적중률을 높이지 못합니다. 재사용할 설계는 분해 → 작업 정의 → 추론이며, 각 작업은 대상, 경계, 증거, 완료 조건, 검증자, 국소적 개선 규칙으로 고정되어야 합니다.

- [2026-06-26 — ClickHouse — AI로 취약점을 찾는 방법](https://clickhouse.com/blog/how-i-hunt-for-vulnerabilities-with-ai)
  - 대형 C++ 시스템에서 사람이 검토에 참여하는 현실적인 워크플로를 배울 수 있습니다. LLM으로 구조를 탐색하고, 가설을 만들고, dataflow를 추적하고, PoC를 작성하되, 심각해 보이는 후보도 상당수는 틀릴 수 있다고 가정합니다. 자동화 시스템은 로컬 재현, 컨테이너 기반 실행 환경, 정확한 trigger condition, maintainer가 이해할 수 있는 충분한 증거를 요구해야 합니다.

- [2026-06-25 — pkqs91 / Octane Security — Codex로 3개월 만에 $200k를 번 방법](https://x.com/pkqs91/status/2070157806104457395)
  - X 아티클이 설명하는 실제 버그바운티 흐름을 배울 수 있습니다. intake 단계에서는 범위, 심각도 규칙, 제외 조건, 문서, 코드를 로컬 분석 묶음으로 바꾸고, hunting 단계에서는 넓게 탐색하며, verification 단계에서는 범위 밖 후보, 방어 로직 때문에 불가능한 경로, 영향이 없는 실마리를 제거합니다. 핵심 규칙은 “넓게 탐색하고, 깊게 악용 가능성을 검증하라”이며, 영향 필터를 통과한 소수만 수동 재현해야 합니다.

- [2026-06-24 — Asymmetric Research — Under the Hood: Engineering Commonware Fuzzing](https://blog.asymmetric.re/under-the-hood/)
  - 심각한 버그 헌팅에서 fuzzing을 지속적인 검증 인프라로 운영하는 법을 배울 수 있습니다. deterministic runtime, controlled randomness, 검토 가능한 fuzz target, 재현 가능한 crash, regression 승격이 핵심입니다. Commonware는 일반적인 Web3 앱 플랫폼이라기보다 blockchain, consensus, storage, networking, cryptographic primitive로 구성된 Rust stack이며, 재사용할 교훈은 LLM이 만든 가설도 deterministic harness를 통과해야 finding이 된다는 점입니다.

- [2026-06-23 — Zero Cool Labs — Symmetry Sniper](https://x.com/ZeroCool_AI/status/2069443859327705497)
  - X 아티클의 좁은 범위 전문 스킬 패턴을 배울 수 있습니다. 문서, 이름, 이벤트, 인터페이스, 테스트에서 의도된 대칭 관계를 먼저 확인한 뒤, 상태 변경, 자산 이동, 권한 검사, 반올림, 0 값, 경계값 처리를 비교합니다. 보고 가능한 취약점은 불변식 위반, 권한 불일치, 수수료/반올림 비대칭, batch 우회처럼 측정 가능한 영향만 허용하므로 잡음이 적은 스킬 설계에 좋습니다.

- [2026-06-18 — Cloudflare — 나만의 취약점 하네스 만들기](https://blog.cloudflare.com/build-your-own-vulnerability-harness/)
  - 운영 환경에 가까운 취약점 탐색 파이프라인을 배울 수 있습니다. 정찰, 탐색, 빈 영역 보강, 추적, 중복 제거, 판단, 수정, 여러 하네스의 결과를 받아들이는 검증 시스템이 핵심입니다. 특히 단계별 상태 저장, 좁은 조사를 위한 micro-fork, 필요한 자료를 요청하는 wishlist, 저장소 간 추적, 검증되지 않은 실마리가 보고서로 넘어가지 못하게 하는 gate를 참고할 만합니다.

- [2026-06-17 — Praetorian — FreeBSD Kernel에서의 AI 취약점 연구](https://www.praetorian.com/blog/ai-vulnerability-research-freebsd-kernel/)
  - 커널 감사를 실행 가능한 검증 루프로 바꾸는 방법을 배울 수 있습니다. 소스 리뷰, 가설 수립, trigger program, 계측된 VM, KASAN telemetry, 재현 코드 반복 수정이 핵심입니다. 심각한 native-code 헌팅에서는 검증 환경이 특히 중요하며, 에이전트가 실패를 관찰하고 sanitizer 출력을 읽고 PoC를 고치거나 잘못된 후보를 버릴 수 있어야 합니다.

- [2026-06-01 — Night-Wolf — Closed Source Software에서의 AI-Powered Bug Hunting](https://blogs.night-wolf.io/ai-powered-bug-hunting-in-closed-source-software)
  - 소스코드가 없을 때 에이전트 기반 헌팅을 바꾸는 방법을 배울 수 있습니다. 디컴파일하고, 디컴파일러용 스킬을 만들고, 여러 관점의 검토를 돌리고, 불가능한 전제조건을 걸러내며, 블랙박스 검증으로 후보를 확인합니다. 리버싱 산출물은 불완전한 증거이므로 실제 바이너리에서 주장한 공격 경로가 존재한다는 런타임 확인이 필요합니다.

- [2026-06-01 — se1en — AI 기반 취약점 탐지 워크플로우 구축기](https://se1en.tistory.com/15)
  - 실전에서 나온 세 단계 흐름을 배울 수 있습니다. 대상 전처리, 취약점 가설 생성, 후보 검증을 분리합니다. 미묘한 교훈은 정책 맥락입니다. 많은 취약점 후보가 회색지대에 있기 때문에, 에이전트는 코드 패턴을 곧바로 악용 가능하다고 부르기 전에 서비스 의도와 책임 경계를 알아야 합니다.

- [2026-05-27 — Night-Wolf — Reasoning-First 취약점 연구로 오픈소스 프로젝트에서 여러 버그 찾기](https://blogs.night-wolf.io/reasoning-first-vulnerability-research-that-found-multiples-bugs-in-open-source-project)
  - 글이 제시하는 명시적 루프를 배울 수 있습니다. MODEL로 대상을 모델링하고, HYPOTHESIZE로 공격자가 제어하는 입력과 깨지는 가정을 세우며, INVESTIGATE로 코드와 dataflow를 읽고, CONFIRM으로 최소 로컬 PoC를 만들고, RECORD하거나 버립니다. 에이전트가 추론에서 증거로 넘어가도록 강제하는 좋은 틀입니다.

- [2026-05-24 — Monad — Monad Bugfinder](https://www.monad.xyz/blog/monad-bugfinder)
  - Mythos와 TxRay에서 영감을 받은 Web3 작업 흐름을 배울 수 있습니다. 발견과 검증을 분리하고, 모든 실마리를 구조화된 데이터베이스에 저장하며, triage를 후보 생성과 별도 작업으로 취급합니다. 진지한 자율 헌팅에는 채팅 기록에 의존하는 방식이 아니라 가설, 증거, 중복 여부, 공격 시도, 검토자 판단을 담는 지속적인 기록원이 필요합니다.

- [2026-05-12 — Microsoft — MDASH](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/)
  - MDASH의 구체적인 단계를 배울 수 있습니다. 언어별 index와 threat model을 준비하고, 감사 담당 에이전트가 스캔하며, 토론 담당 에이전트가 도달 가능성과 악용 가능성을 검토하고, 의미상 중복을 제거한 뒤, ASan 사례 같은 동적 트리거로 증명합니다. 핵심은 한 에이전트가 모든 일을 하는 것이 아니라, 100개 이상의 전문 에이전트가 반박 실패와 증거 산출물을 통해 신뢰도를 높인다는 점입니다.

- [2026-05-04 — Asymmetric Research — Introducing Crucible: Solana를 위한 Invariant Fuzzing Framework](https://blog.asymmetric.re/introducing-crucible-an-invariant-fuzzing-framework-for-solana/)
  - Crucible 자체는 agentic system이 아니지만, 논리 버그 에이전트에 정밀한 의미론적 목표를 주는 방법을 배울 수 있습니다. 도메인 추론을 실행 가능한 invariant로 만들고, 상태를 가진 Solana action을 모델링하고, typed instruction sequence에서 위반 witness를 탐색합니다. delegation weight가 backing lamports를 초과한 phantom-stake 사례는 LLM이 만든 경제·합의 invariant가 일반적인 퍼징이 아니라 다단계 검증을 이끄는 방식을 보여줍니다.

- [2026-04-29 — Xint — Copy Fail: 주요 Linux 배포판에서 732바이트로 root 획득](https://xint.io/blog/copy-fail-linux-distributions)
  - 날카로운 불변식 하나와 에이전트식 탐색 루프가 치명적인 커널 버그를 드러내는 방식을 배울 수 있습니다. “page cache에 기반한 데이터가 writable crypto scatterlist에 들어가면 안 된다”는 구체적인 의심을 주고, 에이전트가 관련 조합 공간을 변종 탐색하게 한 뒤, 얻어진 공격 능력과 재현 가능한 공격 경로로 심각도를 판단하는 패턴이 핵심입니다.

- [2026-04-14 — AISLE — System Over Model: Jagged Frontier에서의 Zero-Day 발견](https://aisle.com/blog/system-over-model-zero-day-discovery-at-the-jagged-frontier)
  - 더 강한 모델 하나보다 넓은 coverage, 저렴한 병렬 처리, 회의적인 검토가 더 효과적일 수 있음을 배울 수 있습니다. 파일별 보안 문맥을 만들고, 함수 단위로 스캔한 뒤, 여러 차례 검토로 후보를 반박해봅니다. 예산이 제한된 심각한 취약점 헌팅에서는 먼저 더 많은 코드를 보게 하고, 반박을 견딘 후보에만 깊은 추론을 쓰는 편이 좋습니다.

- [2026-04-09 — Asymmetric Research — Understanding Agents: Coding Agent를 위한 Code Coverage](https://blog.asymmetric.re/understanding-agents-code-coverage-for-coding-agents/)
  - coding agent의 관측성을 배울 수 있습니다. 세션 trace를 분석해 에이전트가 실제로 읽은 line range와 의도를 표시하고, prompt, model, reasoning effort를 구체적인 code coverage로 비교합니다. 버그 헌팅에서는 긴 대화 기록을 의미 있는 coverage로 착각하지 말고, 에이전트가 보지 않은 blind spot을 다시 탐색하게 만드는 산출물로 쓸 수 있습니다.

- [2026-04-08 — Plamen Tsanev — BEAST post](https://x.com/p_tsanev/status/2041880807032119670)
  - “몇 개의 에이전트가 전체 코드베이스를 읽게 하는” 첫 실험이 왜 실패했고, Plamen이 왜 전문화된 재귀 구조로 이동했는지 배울 수 있습니다. 글은 정찰, 초기 설정, 넓은 탐색, 인벤토리/의미 분석, 깊은 분석, 체인 분석, PoC, 보고서의 8단계 흐름을 제시합니다. 인간의 피로 때문에 놓치기 쉬운 복합 logic bug를 찾는 데 특히 유용합니다.

- [2026-04-07 — Anthropic — Mythos: Claude Mythos Preview의 사이버보안 역량 평가](https://www.anthropic.com/research/mythos-preview)
  - Mythos Preview의 취약점 탐색 루프를 배울 수 있습니다. 파일별 버그 가능성을 먼저 매기고, 서로 다른 파일에 병렬 에이전트를 배치하며, PoC 익스플로잇과 재현 절차가 포함된 보고서를 요구합니다. 핵심은 넓은 탐색과 최종 검증을 분리하고, 여러 집중 탐색이 만든 후보를 실행 가능한 공격 증거로 다시 걸러내는 것입니다.

- [2026-03-25 — Security Cryptography Whatever — Nicholas Carlini와 AI Bug Finding](https://securitycryptographywhatever.com/2026/03/25/ai-bug-finding/)
  - AI 기반 버그 발견을 확장할 때 생기는 운영상 병목을 배울 수 있습니다. 재사용 가능한 하네스는 많은 후보를 빠르게 찾을 수 있지만, 검증, 책임 있는 제보, maintainer가 바로 행동할 수 있는 보고서 작성이 병목이 됩니다. 처리량을 높이려면 적극적인 중복 제거, 보고서 품질 관리, 심각한 후보에 대한 사람 검토 예산, 수정 비용 측정이 필요합니다.

- [2026-03-24 — Anthropic — 장기 실행 애플리케이션 개발을 위한 하네스 설계](https://www.anthropic.com/engineering/harness-design-long-running-apps)
  - 장시간 실행되는 에이전트가 흐트러지지 않게 만드는 하네스 설계를 배울 수 있습니다. 생성 담당과 평가 담당을 분리하고, 평가 기준을 구체화하며, 작업을 작은 단위로 나누고, 구조화된 산출물과 인수인계가 포함된 문맥 초기화를 사용합니다. 버그 헌팅에서는 이를 탐색 담당/검증 담당 분리, 증거 파일, 재시작 가능한 상태 관리로 옮겨와야 약한 후보가 그대로 확정되는 일을 줄일 수 있습니다.

- [2026-03-23 — Yue Xue — AI Auditing Methodology, Part II](https://x.com/xy9301/status/2036017855381340269)
  - 글에서 말하는 자기 개선의 조건인 variation, selection, inheritance를 배울 수 있습니다. 버그 헌팅 시스템에서는 새 프롬프트, 스킬, 분해 방식을 시도하고, benchmark나 사람 검토로 평가하며, 되돌릴 수 있는 경험을 유지해야 합니다. 중복 제거, 검증, PoC 생성은 부가 기능이 아니라 작업 흐름의 핵심 단계로 다뤄야 합니다.

- [2026-03-16 — OpenAI — Codex Security가 SAST 보고서를 포함하지 않는 이유](https://openai.com/index/why-codex-security-doesnt-include-sast/)
  - Codex Security의 더 구체적인 설계 관점을 배울 수 있습니다. 단순히 source-to-sink 경고를 먼저 쌓지 않고, 실제 동작, 제약 조건, 도달 가능한 공격 경로, sandbox 검증에서 출발합니다. 에이전트 기반 버그 헌팅에서는 공격자 입력, 방어 가정의 실패, 가능한 실행 증거, 기존 의도를 깨지 않는 패치를 모두 요구하는 필터로 참고할 만합니다.

- [2026-03-15 — Yue Xue — AI Auditing Methodology, Part I](https://x.com/xy9301/status/2033186266649640980)
  - X 아티클의 핵심 주장을 배울 수 있습니다. AI 보안은 프레임워크 중심 파이프라인에서 코딩 에이전트 중심 흐름으로 이동하고 있으며, 먼저 인간의 감사 과정을 재현한 뒤 자동화하는 방향으로 바뀌고 있습니다. 진지한 헌터를 만들려면 모든 것을 무거운 정적 프레임워크로 전처리하기보다, 풍부한 프로젝트 자료 위에서 유연하게 탐색을 시작하는 편이 좋습니다.

- [2026-03-12 — Asymmetric Research — 실제로는 취약점이 아닌 Solana 오탐 사례들](https://blog.asymmetric.re/solana-vulnerabilities-that-arent-unpacking-common-misreports/)
  - LLM 감사자를 위한 Solana 특화 false-positive guardrail을 배울 수 있습니다. 널리 알려진 “취약점” 중 상당수는 오래되었거나 잘못 전파된 내용이므로, 에이전트는 runtime version, framework behavior, exploit precondition, 실제 impact를 확인해야 합니다. 논리 버그 헌터를 만들 때 오래된 보안 지식은 그럴듯한 추론처럼 보일 수 있으므로, 하네스가 환경에 맞는 검증을 강제해야 합니다.

- [2026-03-09 — Devansh — Needle in the Haystack: 취약점 연구를 위한 LLM](https://devansh.bearblog.dev/needle-in-the-haystack/)
  - 막연한 “버그 찾아줘”식 프롬프트가 왜 실패하는지 배울 수 있습니다. threat model, 공격자 능력, 신뢰 경계, 과거 CVE 맥락이 없으면 모델은 CWE 이름만 닮은 환각성 지적을 많이 만듭니다. 재사용할 기본 절차는 작게 시작하는 것입니다. 얇은 범위를 고르고, 한 페이지 threat model을 만들고, 불변식과 진입점을 찾은 뒤 악용 가능성을 증명하는 call chain과 테스트를 요청합니다.

- [2026-02-11 — OpenAI — Harness Engineering](https://openai.com/index/harness-engineering/)
  - 에이전트가 저장소와 실행 환경을 제대로 이해하도록 만드는 방법을 배울 수 있습니다. AGENTS.md를 작업 지도처럼 쓰고, 저장소 내부 문서를 기준 정보로 관리하며, lint/CI로 문서와 코드의 일관성을 강제하고, worktree별 격리 실행 환경과 DevTools, 로그, 메트릭, 트레이스를 제공합니다. 버그 헌팅에서도 자율 실행 전에 코드 지도, threat model, 검증 명령, 관찰 가능한 실행 환경을 먼저 갖춰야 합니다.

- [2025-12-01 — Anthropic — AI 에이전트가 $4.6M 규모의 스마트 컨트랙트 익스플로잇을 발견](https://www.anthropic.com/research/smart-contracts)
  - 도메인 전용 실행 환경이 에이전트의 품질을 어떻게 바꾸는지 배울 수 있습니다. MCP로 Foundry/anvil/cast 도구를 제공하고, 특정 블록의 체인 상태를 fork해 재현 가능한 테스트 환경을 만들며, 실제 익스플로잇 성공을 명확한 성공 기준으로 삼습니다. Web3 헌팅에서는 경제적 영향이 있는 PoC만 보고하고, 실패한 트랜잭션은 다음 시도를 개선하는 피드백으로 써야 합니다.

- [2025-11-04 — Seokchan Yoon — $5짜리 프롬프트로 $2,418짜리 취약점 찾은 썰](https://new-blog.ch4n3.kr/llm-found-security-issues-from-django-ko/)
  - 완전 자율 하네스가 아니라 가벼운 LLM 후보 채굴 baseline을 배울 수 있습니다. 취약점 종류를 Django DoS로 좁히고, 1-day CVE 예시와 정책 맥락을 프롬프트에 넣고, 관련 소스 파일을 XML로 묶어 토큰 예산 안에서 전달한 뒤, false positive가 많은 후보를 모아 triage합니다. 저비용 넓은 탐색이 놓친 패턴을 드러낼 수 있지만, 진지한 제보에는 도달 가능성, 영향, 정책 검증이 필요하다는 점이 핵심입니다.

- [2025-09-04 — Team Atlanta — Harness에서 Vulnerability까지: 코드 이해와 버그 발견을 위한 AI 에이전트](https://team-atlanta.github.io/blog/post-mlla-disc-agents/)
  - AIxCC 규모의 에이전트가 거대한 코드베이스를 실제 공격 실마리로 좁히는 방식을 배울 수 있습니다. harness entrypoint 식별, 정확한 함수 검색, 재귀적 call graph 구성, 오염된 인자 추적, 위험 sink 표시가 핵심입니다. 익스플로잇 생성은 공격자가 제어하는 데이터가 보안상 중요한 연산에 도달한다는 점이 확인된 뒤에 시작해야 합니다.

- [2025-08-08 — Trail of Bits — Buttercup 오픈소스 공개](https://blog.trailofbits.com/2025/08/08/buttercup-is-now-open-source/)
  - 상위권 AIxCC 시스템의 전체 취약점 발견 구조를 배울 수 있습니다. AI 보강 입력 생성, 문맥 기반 정적 분석, PoV 중복 제거, 패치 생성, 검증이 핵심입니다. 자율 헌터를 만들 때는 fuzzing, LLM 추론, 중복 관리, 수정 루프를 한 에이전트의 결론에만 의존하지 않고 조율하는 구조로 참고할 수 있습니다.

- [2025-08-08 — Theori / Xint — AI 해커를 만들며 배운 것](https://theori.io/ko/blog/building-effective-llm-agents-63446)
  - Theori의 실전 에이전트 설계 원칙을 배울 수 있습니다. 사람이 휴리스틱으로 푸는 작업에 에이전트를 쓰고, 취약점 탐지, PoV 생성, 패치 작성, 원인 분석을 좁은 역할로 나누며, 각 역할에 필요한 도구만 제공합니다. 재사용할 만한 세부 요소는 구조화된 출력, 종료 전용 도구, 필수 도구 호출, 검증 피드백, 진행 상황 개입입니다.

- [2025-07-03 — Gustavo Grieco — Quimera 소개](https://gustavo-grieco.github.io/blog/introducing-quimera/)
  - Ethereum contract를 위한 피드백 기반 익스플로잇 생성을 배울 수 있습니다. 소스코드, 온체인 맥락, Foundry trace, Solidity PoC 반복을 “자금 탈취” 같은 단일 고영향 목표에 맞춰 결합합니다. 취약점 종류를 좁히고, 실행 가능한 성공 기준을 연결하고, trace 실패가 다음 전략을 이끌게 해야 막연한 감사 코멘트에 머물지 않습니다.

- [2024-11-01 — Google Project Zero — From Naptime to Big Sleep](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html)
  - Big Sleep의 첫 공개 사례를 배울 수 있습니다. 실제 코드에서 LLM이 변종 취약점을 찾고, 실패한 테스트케이스를 고치며, 크래시를 재현하고, maintainer가 조치할 수 있는 원인 분석 보고서를 작성하는 흐름입니다. 가설 → 테스트케이스 → 근본 원인으로 이어지는 피드백 루프를 가져오되, 최신 운영 현황이라기보다는 초기 사례 연구로 보는 편이 좋습니다.

## 논문
- [2026-07-08 — Thinking More, Harnessing Better: Project Digestion과 Workflow Decomposition을 이용한 Harness 자동 생성](https://arxiv.org/abs/2607.07007)
  - 대상이 퍼징이 아니더라도 SynapseFlow의 단계적 하네스 생성 workflow를 배울 수 있습니다. 프로젝트를 structural flow graph로 소화하고, 관련 함수 triplet을 묶은 뒤, correctness가 깨지면 rollback하는 4단계 프로세스로 하네스를 합성합니다. 재사용할 핵심은 one-shot 생성에 기대지 않고, 하네스 생성을 분해 가능하고 측정 가능하며 실패 복구 가능한 과정으로 만드는 것입니다.

- [2026-07 — Thought Is All You Need: Thought-Augmented LLM을 이용한 Smart Contract 취약점 탐지](https://yajin.org/papers/fse2026_synapse.pdf)
  - Synapse의 사고 템플릿 기반 스마트 컨트랙트 감사 루프를 배울 수 있습니다. 감사 보고서에서 재사용 가능한 취약점 추론 사고를 추출하고, 이를 대상 코드의 핵심 맥락에 맞게 구체화한 뒤, Developer, Researcher, Auditor, Verifier 에이전트와 의미 기반 도구로 검증합니다. 고정 패턴 스캐너나 코드 의미를 이해하지 못하는 퍼징 오라클이 놓치기 쉬운 상태 업데이트 누락과 계약 의미 기반 버그를 겨냥한다는 점에서 논리 버그 헌팅에 특히 유용합니다.

- [2026-07-01 — Knowdit — Auditing Knowledge Summarization을 이용한 Agentic Smart Contract Vulnerability Detection](https://arxiv.org/html/2603.26270v2)
  - 논문 기반 지식 주도 스마트 컨트랙트 감사를 배울 수 있습니다. 인간 감사 보고서에서 감사 지식 그래프를 만들고, DeFi 의미와 버그 패턴을 대상에 대응시킨 뒤, 명세 생성, PoC 합성, 실행, 회고를 반복합니다. 보고된 high 9개와 medium severity unknown finding 36개는 도메인 지식과 실행 가능한 증거가 일반적인 Solidity 냄새 탐지보다 강하다는 점을 보여줍니다.

- [2026-06-20 — Revelio: Repository-Scale Codebase를 위한 비용 효율적 Agentic Memory Safety 취약점 탐지](https://arxiv.org/abs/2606.22263)
  - 저장소 단위 에이전트를 위한 hallucination 방지 검증 게이트를 배울 수 있습니다. 저렴한 LLM과 가벼운 정적 분석으로 가설을 만들고 순위를 매기지만, 최종 finding은 실행 가능한 Proof-of-Vulnerability가 deterministic sanitizer에서 재현될 때만 살아남습니다. 논리 버그보다는 memory safety 중심이지만, generate-rank-prove 계약은 심각한 버그 헌팅 시스템 전반에 적용할 만한 강한 패턴입니다.

- [2026-06-17 — Code-Augur: Specification Inference를 이용한 Agentic Vulnerability Detection](https://arxiv.org/abs/2606.18619)
  - 에이전트가 코드를 안전하다고 판단할 때 사용한 가정을 in-source invariant로 외부화하고, 런타임에서 이를 반증하고 개선하는 specification-first loop를 배울 수 있습니다. 구현은 반증 수단으로 guided fuzzing을 사용하지만, 논리 버그 헌팅에 옮길 핵심은 에이전트의 안전성 판단을 그대로 믿지 않고 암묵적 추론을 실행 가능한 의무로 바꾸는 것입니다.

- [2026-05-28 — Agora: LLM Agent를 이용한 Production-Level Consensus Protocol 자율 버그 탐지](https://arxiv.org/abs/2605.29910)
  - 공개된 사례 중 agentic consensus logic bug hunting에 가장 가까운 workflow를 배울 수 있습니다. 도메인 인식 에이전트가 프로토콜 상태를 유지하고, fault model 제약을 적용하고, 공격 시나리오를 합성하며, global safety invariant에 대해 가설을 반복 검증합니다. Raft, EPaxos, HotStuff, BullShark에서 보고한 15개의 알려지지 않았던 protocol-level logic bug는 고립된 코드 리뷰보다 상태 기반 시나리오 추론과 hypothesis-driven testing이 중요한 이유를 보여줍니다.

- [2026-05-12 — VulTriage: LLM 기반 취약점 탐지를 위한 Triple-Path Context Augmentation](https://arxiv.org/abs/2605.09461)
  - 미묘한 의미 차이로 갈리는 취약점을 위한 context packing 단계를 배울 수 있습니다. Control Path는 AST, CFG, DFG 정보를 말로 풀어주고, Knowledge Path는 CWE 기반 패턴과 예시를 검색하며, Semantic Path는 최종 판단 전에 코드 동작을 요약합니다. 완전한 자율 에이전트는 아니지만, 리뷰어에게 control-flow, data-flow, 사전 지식, 동작 요약을 구조화해서 제공하는 하네스 구성요소로 유용합니다.

- [2026-04-22 — Synthesizing Multi-Agent Harnesses for Vulnerability Discovery](https://arxiv.org/abs/2604.20801)
  - 프롬프트뿐 아니라 하네스 자체를 최적화하는 방법을 배울 수 있습니다. 역할, 도구, 통신 구조, 재시도, 조율 규칙을 typed graph DSL로 모델링합니다. 핵심 교훈은 대상에서 나온 런타임 피드백으로 어떤 하네스 구성요소가 실패했는지 진단하고, 체계적으로 다시 설계할 수 있다는 점입니다. 논문은 알려지지 않았던 Chrome zero-day 발견도 보고합니다.

- [2026-03-28 — VulInstruct: Security Specification으로 LLM에 취약점 근본 원인 추론을 가르치기](https://arxiv.org/abs/2511.04014)
  - 명세 기반 근본 원인 추론을 배울 수 있습니다. 과거 패치와 저장소별 반복 위반에서 안전 동작 기대치를 추출하고, 관련 명세와 사례를 검색한 뒤, 모델이 코드 동작을 기대 보안 속성과 비교해 판단하게 합니다. CWE 패턴 매칭에서 벗어나 invariant/specification 불일치와 명시적 추론으로 이동한다는 점에서 논리 버그 헌팅에 유용합니다.

- [2026-02-10 — QRS: Autonomous Vulnerability Discovery를 위한 Rule-Synthesizing Neuro-Symbolic Triad](https://arxiv.org/abs/2602.09774)
  - Query, Review, Sanitize 하네스 구성을 배울 수 있습니다. 에이전트가 schema와 예시에서 CodeQL query를 합성하고, 발견 결과를 의미론적으로 검토하며, 후보를 자동 익스플로잇 합성까지 밀어붙입니다. CodeQL이 포함되어 있긴 하지만 핵심은 정해진 SAST 규칙이 아니라 에이전트가 만든 검사를 독립적인 의미 검증과 익스플로잇 관점의 검증으로 걸러내는 구조입니다.

- [2026-02-03 — LogicScan: Smart Contract Business Logic 취약점 탐지를 위한 LLM-driven Framework](https://arxiv.org/abs/2602.03271)
  - 스마트 컨트랙트 비즈니스 로직 버그 워크플로를 배울 수 있습니다. 성숙한 배포 프로토콜에서 invariant를 채굴하고, 이를 Business Specification Language로 정규화한 뒤, 대상 컨트랙트를 기준 제약과 대조합니다. 목표가 reentrancy나 산술 오류 같은 고정 시그니처가 아니라 누락되었거나 약하게 강제된 business invariant라는 점에서 논리 버그에 직접 맞닿아 있습니다.

- [2026-01-27 — AgenticSCR: Immature Vulnerability 탐지를 위한 Autonomous Agentic Secure Code Review](https://arxiv.org/abs/2601.19138)
  - pre-commit 단계의 보안 코드 리뷰 에이전트 설계를 배울 수 있습니다. 자율적인 코드 탐색, 도구 호출, 보안 중심 의미 메모리를 사용해 맥락 의존적인 immature vulnerability를 찾습니다. 잡음 많은 SAST 결과를 나중에 보는 대신, 위험한 변경이 production bug로 굳기 전에 리뷰 코멘트가 위치, 설명, 반박 포인트를 제공하게 만드는 것이 핵심입니다.

- [2026-01-12 — AI Agent Smart Contract Exploit Generation](https://arxiv.org/html/2507.05558v2)
  - A1의 실행 중심 익스플로잇 모델을 배울 수 있습니다. LLM에 도메인 도구를 제공하고, 공격 전략을 생성하고, fork된 체인 상태에서 Solidity PoC를 실행하며, 실행 피드백으로 전략을 다듬습니다. 논문의 핵심 원칙은 “구체적으로 검증된 수익성 있는 PoC 익스플로잇만 보고한다”는 점이며, 62.96% VERITE 성공률은 Web3 에이전트 출력의 강한 기준으로 유용합니다.

- [2025-12-08 — VulnLLM-R: 취약점 탐지를 위한 Specialized Reasoning LLM과 Agent Scaffold](https://arxiv.org/abs/2512.07533)
  - 취약점 추론 모델을 프로젝트 단위 헌팅 에이전트 scaffold로 감싸는 방식을 배울 수 있습니다. 프로그램 상태를 추론하고, 추론 시점 최적화를 적용하며, 실제 저장소에서 CodeQL과 AFL++ baseline을 기준으로 검증합니다. LogicScan이나 Synapse만큼 논리 버그에 특화된 것은 아니지만, 모델 특화, 에이전트 scaffold, 실제 finding 검증을 분리해 보는 데 유용합니다.

- [2025-10-04 — RFCAudit: Network Protocol Functional Bug 탐지를 위한 LLM Agent](https://arxiv.org/abs/2506.00714)
  - 네트워크 프로토콜의 명세와 구현 사이 semantic drift를 탐지하는 방법을 배울 수 있습니다. indexing agent가 코드 동작을 계층적으로 요약하고, detection agent가 필요한 자료구조와 함수를 demand-driven 방식으로 검색해 RFC 요구사항과 대조합니다. 발견한 functional bug 47개 중 20개가 개발자에게 확인되거나 수정됐다는 결과는 protocol specification과 invariant를 노드 클라이언트 동작에 대조하는 모델로 유용합니다.

- [2025-08 — LLMxCPG: Code Property Graph 기반 Context-Aware 취약점 탐지](https://www.usenix.org/conference/usenixsecurity25/presentation/lekssays)
  - 저장소 단위 에이전트를 위한 context reducer를 배울 수 있습니다. CPG 기반 slice를 만들어 function-level과 multi-function 사례에서 취약점 관련 맥락을 보존하면서 코드 크기를 줄입니다. 그 자체로 agentic workflow는 아니지만, LLM 리뷰어가 어떤 코드, 의존성, 문법 변형을 봐야 하는지 결정하는 pre-agent 계층으로 실용적입니다.

- [2025-07 — Code Repository 취약점 탐지를 위한 LLM/LLM Agent 벤치마킹](https://aclanthology.org/2025.acl-long.1490/)
  - 저장소 단위 에이전트를 평가하는 하네스를 배울 수 있습니다. JITVul은 vulnerable function을 취약점을 도입한 커밋과 고친 커밋에 연결해, 에이전트가 고립된 함수가 아니라 interprocedural, multi-hop 맥락을 다루게 만듭니다. ReAct agent 결과는 맥락과 도구 사용이 좋아져도 과잉 분석과 guard 오독이 남는다는 점을 보여주므로, 버그 헌팅 하네스의 현실성 검증에 유용합니다.

- [2024-09-14 — iAudit: Fine-Tuning과 LLM-based Agent를 결합한 Smart Contract 감사](https://arxiv.org/abs/2403.16073)
  - iAudit의 2단계 스마트 컨트랙트 감사 루프를 배울 수 있습니다. Detector가 취약해 보이는 코드를 찾고, Reasoner가 원인을 제시한 뒤, Ranker와 Critic 에이전트가 어떤 원인이 증거를 가장 잘 설명하는지 토론합니다. 논리적 스마트 컨트랙트 finding에는 단순한 vulnerable/not-vulnerable 라벨보다 원인 선택과 적대적 정당화가 필요하다는 점이 핵심입니다.

## 프로젝트
- [lebronlambert — Agora](https://github.com/lebronlambert/Agora)
  - orchestrator, 프로토콜 인식 전략 생성, 반복적인 테스트 생성과 reflection, persistent knowledge, 확인된 버그의 variant mining으로 구성된 실행 가능한 consensus bug hunter를 살펴볼 수 있습니다. 단일 함수 취약점 패턴보다 상태 의존적인 consensus safety violation을 겨냥하는 에이전트 루프로 확장하기에 이 목록에서 가장 직접적인 프로젝트입니다.

- [Asymmetric Research — agent-coverage](https://github.com/asymmetric-research/agent-coverage)
  - Codex와 Claude Code session log를 파일·라인 coverage로 바꾸고, 각 코드 접근을 해당 task나 subagent에 연결한 뒤, local checkout 위에서 blind spot을 시각화하는 가벼운 observability prototype입니다. 이를 semantic coverage나 bug coverage가 아닌 attention coverage로 취급하고, 비어 있는 영역에 다음 감사 실행을 배정하거나 prompt와 model의 대상 탐색 방식을 비교하는 데 사용할 수 있습니다.

- [Cloudflare — security-audit-skill](https://github.com/cloudflare/security-audit-skill)
  - 간결한 6단계 코딩 에이전트 감사 스킬을 배울 수 있습니다. 병렬 정찰, 여러 관점의 탐색, 적대적 검증, 중복 제거, 누적 결과, 기계가 읽을 수 있는 출력이 포함됩니다. 가장 중요한 재사용 규칙은 검증 담당을 발견 담당과 분리하고, 후보가 살아남기 전에 악용 가능성을 적극적으로 반박하게 하는 것입니다.

- [Visa — Visa Vulnerability Agentic Harness](https://github.com/visa/visa-vulnerability-agentic-harness)
  - 특정 벤더에 묶이지 않는 SAST 하네스를 배울 수 있습니다. threat modeling, 여러 관점의 분석, 결정론적 투표, 적대적 검증, 구조화된 Markdown/SARIF 결과, remediation, validation이 포함됩니다. 기업 시스템에서는 AI가 발견한 악용 가능성부터 검증된 수정까지 전체 수명주기를 최적화한다는 점이 특히 유용합니다.

- [Anthropic — Defending Code Reference Harness](https://github.com/anthropics/defending-code-reference-harness)
  - C/C++ 메모리 버그를 위한 정찰 → 발견 → 검증 → 보고 → 패치 reference implementation을 배울 수 있습니다. Docker, ASAN, gVisor 격리, 병렬 에이전트가 포함됩니다. `.claude/skills`의 quickstart, threat-model, vuln-scan, triage, patch, customize 템플릿도 함께 보면 실행 가능한 하네스와 재사용 가능한 보안 작업 스킬 계층을 동시에 배울 수 있습니다.


- [AISLE — nano-analyzer](https://github.com/weareaisle/nano-analyzer)
  - LLM 기반 native-code scanner의 가장 작은 유용한 버전을 배울 수 있습니다. 파일별 문맥 생성, 취약점 스캔, 회의적 triage, 최종 판정이 핵심입니다. 추가적인 에이전트 기능이 실제로 가치 있는지 비교할 수 있는 저비용 coverage-first 기준선으로 좋습니다.

- [PlamenTSV — plamen](https://github.com/PlamenTSV/plamen)
  - BEAST 아이디어의 실행 가능한 버전을 배울 수 있습니다. 18–100개의 Claude/Codex 에이전트가 8단계로 동작하며, 스마트 컨트랙트와 L1 노드 클라이언트 인프라에 대해 검증된 PoC 익스플로잇이 포함된 감사 보고서를 생성합니다. PTY로 감독되는 worker, 디스크 산출물을 기준 정보로 삼는 방식, 재개 가능한 checkpoint, PoC pass/fail 표시, 남은 의무를 드러내는 방식이 주요 구현 세부사항입니다.

- [berabuddies — AgentFlow](https://github.com/berabuddies/agentflow)
  - 에이전트 기반 버그 헌팅 시스템을 typed graph로 표현하는 법을 배울 수 있습니다. 파일 또는 가설별 fanout, 결과 병합, 검토 실패에 대한 반복 루프, 여러 모델 실행, trace 기반 에이전트 개선이 가능합니다. 수천 개의 제한된 에이전트, 의존성 관계, 공유 작업 공간, 측정 가능한 종료 조건이 필요한 흐름의 구현 패턴으로 볼 만합니다.

- [Trail of Bits — Buttercup](https://github.com/trailofbits/buttercup)
  - OSS-Fuzz 스타일 대상을 찾고 패치하는 실행 가능한 AIxCC 기반 취약점 발견·패치 파이프라인을 배울 수 있습니다. 작업 모니터링, fuzzing, PoV 처리, 패치 생성이 핵심입니다. 전통적 동적 테스트와 LLM 에이전트가 상태를 공유하고, 중복 작업을 피하고, 단순 제안이 아니라 검증된 수정을 만드는 방식을 연구하기 좋습니다.

## 레퍼런스
- [Alin-Mihai BARBATEI — Smart Contract Security Auditing Harness 구축 노트](https://www.linkedin.com/posts/alin-mihai-barbatei-27772b54_notes-on-building-a-smart-contract-security-activity-7476636721527455744-favn)
  - 스마트 컨트랙트 감사 관련 Articles와 workflow 중심 자료를 고를 때 참고했습니다. 이 리스트에서 반복해서 등장하는 codebase context/recon, lead generation, finding identification, issue validation, PoC writing 같은 하네스 단계를 잡는 데 도움이 된 출처입니다.

- [huhusmang — Awesome LLMs for Vulnerability Detection](https://github.com/huhusmang/Awesome-LLMs-for-Vulnerability-Detection)
  - Papers 섹션을 보강할 때 참고했습니다. LLM 기반 취약점 탐지 논문을 넓게 모아 둔 색인으로 사용했고, 그중에서도 agentic bug hunting, workflow 설계, harness 구성, semantic reasoning, exploit validation에 직접 도움이 되는 항목만 선별했습니다.
