# Awesome Agentic Bug Hunting 한국어판

언어: [English](README.md) | 한국어

코딩 에이전트, LLM 기반 하네스, 보안 스킬, 반복 가능한 워크플로를 활용해 실제 소프트웨어 취약점을 찾고 검증하는 공개 자료 모음입니다.

이 리스트는 실전형 에이전트 기반 버그 헌팅에 초점을 둡니다. 소스코드나 바이너리 탐색, 취약점 가설 생성, 검증, PoC 작성, 분류/검토, 재사용 가능한 하네스 설계를 다룹니다.

마지막 검토일: 2026-07-09

## 목차

- [이 리스트에 대하여](#이-리스트에-대하여)
- [아티클](#아티클)
- [프로젝트](#프로젝트)
- [논문](#논문)

## 이 리스트에 대하여

**포함 기준.** 자료는 아래 중 하나 이상을 보여주어야 합니다.

- LLM 또는 에이전트가 직접 관여하는 구체적인 취약점 발견 워크플로.
- 재사용 가능한 하네스, 기본 틀, 에이전트 스킬, 에이전트 아키텍처, 검증 루프.
- 에이전트가 실제 취약점을 찾거나, 검증하거나, 재현하거나, 보고하는 데 도움을 준 사례 연구.
- LLM이 발견, 가설 생성, 검증, 명세 추론에 직접 참여하는 연구 방법.

각 항목은 단순 요약이 아니라, “심각하고 재현 가능한 취약점을 찾는 에이전트 기반 버그 헌팅 시스템에 무엇을 가져올 수 있는가”를 짧게 정리한 학습 노트입니다.

## 아티클

- [Anthropic — 장기 실행 애플리케이션 개발을 위한 하네스 설계](https://www.anthropic.com/engineering/harness-design-long-running-apps)
  - 장시간 실행되는 에이전트가 흐트러지지 않게 만드는 하네스 설계를 배울 수 있습니다. 생성 담당과 평가 담당을 분리하고, 평가 기준을 구체화하며, 작업을 작은 단위로 나누고, 구조화된 산출물과 인수인계가 포함된 문맥 초기화를 사용합니다. 버그 헌팅에서는 이를 탐색 담당/검증 담당 분리, 증거 파일, 재시작 가능한 상태 관리로 옮겨와야 약한 후보가 그대로 확정되는 일을 줄일 수 있습니다.

- [Anthropic — Mythos: Claude Mythos Preview의 사이버보안 역량 평가](https://www.anthropic.com/research/mythos-preview)
  - Mythos Preview의 취약점 탐색 루프를 배울 수 있습니다. 파일별 버그 가능성을 먼저 매기고, 서로 다른 파일에 병렬 에이전트를 배치하며, PoC 익스플로잇과 재현 절차가 포함된 보고서를 요구합니다. 핵심은 넓은 탐색과 최종 검증을 분리하고, 여러 집중 탐색이 만든 후보를 실행 가능한 공격 증거로 다시 걸러내는 것입니다.

- [Anthropic — AI 에이전트가 $4.6M 규모의 스마트 컨트랙트 익스플로잇을 발견](https://www.anthropic.com/research/smart-contracts)
  - 도메인 전용 실행 환경이 에이전트의 품질을 어떻게 바꾸는지 배울 수 있습니다. MCP로 Foundry/anvil/cast 도구를 제공하고, 특정 블록의 체인 상태를 fork해 재현 가능한 테스트 환경을 만들며, 실제 익스플로잇 성공을 명확한 성공 기준으로 삼습니다. Web3 헌팅에서는 경제적 영향이 있는 PoC만 보고하고, 실패한 트랜잭션은 다음 시도를 개선하는 피드백으로 써야 합니다.

- [OpenAI — Harness Engineering](https://openai.com/index/harness-engineering/)
  - 에이전트가 저장소와 실행 환경을 제대로 이해하도록 만드는 방법을 배울 수 있습니다. AGENTS.md를 작업 지도처럼 쓰고, 저장소 내부 문서를 기준 정보로 관리하며, lint/CI로 문서와 코드의 일관성을 강제하고, worktree별 격리 실행 환경과 DevTools, 로그, 메트릭, 트레이스를 제공합니다. 버그 헌팅에서도 자율 실행 전에 코드 지도, threat model, 검증 명령, 관찰 가능한 실행 환경을 먼저 갖춰야 합니다.

- [OpenAI — Codex Security](https://openai.com/index/codex-security-now-in-research-preview/)
  - 애플리케이션 보안 에이전트의 제품형 루프를 배울 수 있습니다. 시스템 맥락을 만들고, 수정 가능한 threat model을 작성하며, 실제 영향 기준으로 우선순위를 매기고, 가능한 경우 sandbox에서 검증한 뒤, 시스템 의도에 맞는 패치를 제안합니다. 심각도는 공격자 능력, 도달 가능성, 프로젝트별 신뢰 경계에 근거해야 하므로 체크리스트식 잡음을 걸러내는 데 유용합니다.

- [Google Project Zero — Project Naptime](https://projectzero.google/2024/06/project-naptime.html)
  - 취약점 연구용 에이전트 인터페이스 설계를 배울 수 있습니다. 정확한 코드 탐색, 참고 자료 조회, Python 기반 입력 생성, debugger 접근, sanitizer 기반 실행, 구조화된 보고가 핵심입니다. 모델에게 무제한 shell을 던져주는 대신 연구자가 실제로 쓰는 도구를 안전한 형태로 제공하고, crash나 런타임 관찰 결과로 진행 상황을 검증해야 합니다.

- [Google Project Zero — From Naptime to Big Sleep](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html)
  - Naptime 아이디어가 실제 변종 취약점 탐색으로 확장되는 과정을 배울 수 있습니다. 범위를 잘 제한한 대상을 고르고, 실패한 테스트케이스를 수정해가며, 크래시를 재현하고, 관리자나 maintainer가 조치할 수 있는 원인 분석 보고서를 작성합니다. 중요한 점은 가설 → 테스트케이스 → 근본 원인으로 이어지는 피드백 경로이며, 대상에 맞춘 fuzzer와 사람의 검토도 여전히 필요하다는 점입니다.

- [Cloudflare — 나만의 취약점 하네스 만들기](https://blog.cloudflare.com/build-your-own-vulnerability-harness/)
  - 운영 환경에 가까운 취약점 탐색 파이프라인을 배울 수 있습니다. 정찰, 탐색, 빈 영역 보강, 추적, 중복 제거, 판단, 수정, 여러 하네스의 결과를 받아들이는 검증 시스템이 핵심입니다. 특히 단계별 상태 저장, 좁은 조사를 위한 micro-fork, 필요한 자료를 요청하는 wishlist, 저장소 간 추적, 검증되지 않은 실마리가 보고서로 넘어가지 못하게 하는 gate를 참고할 만합니다.

- [Team Atlanta — Harness에서 Vulnerability까지: 코드 이해와 버그 발견을 위한 AI 에이전트](https://team-atlanta.github.io/blog/post-mlla-disc-agents/)
  - AIxCC 규모의 에이전트가 거대한 코드베이스를 실제 공격 실마리로 좁히는 방식을 배울 수 있습니다. harness entrypoint 식별, 정확한 함수 검색, 재귀적 call graph 구성, 오염된 인자 추적, 위험 sink 표시가 핵심입니다. 익스플로잇 생성은 공격자가 제어하는 데이터가 보안상 중요한 연산에 도달한다는 점이 확인된 뒤에 시작해야 합니다.

- [Trail of Bits — Buttercup 오픈소스 공개](https://blog.trailofbits.com/2025/08/08/buttercup-is-now-open-source/)
  - 상위권 AIxCC 시스템의 전체 취약점 발견 구조를 배울 수 있습니다. AI 보강 입력 생성, 문맥 기반 정적 분석, PoV 중복 제거, 패치 생성, 검증이 핵심입니다. 자율 헌터를 만들 때는 fuzzing, LLM 추론, 중복 관리, 수정 루프를 한 에이전트의 결론에만 의존하지 않고 조율하는 구조로 참고할 수 있습니다.

- [Xint — 효과적인 LLM 에이전트 만들기 | AI Cyber Challenge](https://xint.io/blog/building-effective-llm-agents-ai-cyber-challenge-165236)
  - Theori의 실전 에이전트 설계 원칙을 배울 수 있습니다. 사람이 휴리스틱으로 푸는 작업에 에이전트를 쓰고, 취약점 탐지, PoV 생성, 패치 작성, 원인 분석을 좁은 역할로 나누며, 각 역할에 필요한 도구만 제공합니다. 재사용할 만한 세부 요소는 구조화된 출력, 종료 전용 도구, 필수 도구 호출, 검증 피드백, 진행 상황 개입입니다.

- [Xint — Copy Fail: 주요 Linux 배포판에서 732바이트로 root 획득](https://xint.io/blog/copy-fail-linux-distributions)
  - 날카로운 불변식 하나와 에이전트식 탐색 루프가 치명적인 커널 버그를 드러내는 방식을 배울 수 있습니다. “page cache에 기반한 데이터가 writable crypto scatterlist에 들어가면 안 된다”는 구체적인 의심을 주고, 에이전트가 관련 조합 공간을 변종 탐색하게 한 뒤, 얻어진 공격 능력과 재현 가능한 공격 경로로 심각도를 판단하는 패턴이 핵심입니다.

- [AISLE — System Over Model: Jagged Frontier에서의 Zero-Day 발견](https://aisle.com/blog/system-over-model-zero-day-discovery-at-the-jagged-frontier)
  - 더 강한 모델 하나보다 넓은 coverage, 저렴한 병렬 처리, 회의적인 검토가 더 효과적일 수 있음을 배울 수 있습니다. 파일별 보안 문맥을 만들고, 함수 단위로 스캔한 뒤, 여러 차례 검토로 후보를 반박해봅니다. 예산이 제한된 심각한 취약점 헌팅에서는 먼저 더 많은 코드를 보게 하고, 반박을 견딘 후보에만 깊은 추론을 쓰는 편이 좋습니다.

- [Microsoft — MDASH](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/)
  - MDASH의 구체적인 단계를 배울 수 있습니다. 언어별 index와 threat model을 준비하고, 감사 담당 에이전트가 스캔하며, 토론 담당 에이전트가 도달 가능성과 악용 가능성을 검토하고, 의미상 중복을 제거한 뒤, ASan 사례 같은 동적 트리거로 증명합니다. 핵심은 한 에이전트가 모든 일을 하는 것이 아니라, 100개 이상의 전문 에이전트가 반박 실패와 증거 산출물을 통해 신뢰도를 높인다는 점입니다.

- [ClickHouse — AI로 취약점을 찾는 방법](https://clickhouse.com/blog/how-i-hunt-for-vulnerabilities-with-ai)
  - 대형 C++ 시스템에서 사람이 검토에 참여하는 현실적인 워크플로를 배울 수 있습니다. LLM으로 구조를 탐색하고, 가설을 만들고, dataflow를 추적하고, PoC를 작성하되, 심각해 보이는 후보도 상당수는 틀릴 수 있다고 가정합니다. 자동화 시스템은 로컬 재현, 컨테이너 기반 실행 환경, 정확한 trigger condition, maintainer가 이해할 수 있는 충분한 증거를 요구해야 합니다.

- [Praetorian — FreeBSD Kernel에서의 AI 취약점 연구](https://www.praetorian.com/blog/ai-vulnerability-research-freebsd-kernel/)
  - 커널 감사를 실행 가능한 검증 루프로 바꾸는 방법을 배울 수 있습니다. 소스 리뷰, 가설 수립, trigger program, 계측된 VM, KASAN telemetry, 재현 코드 반복 수정이 핵심입니다. 심각한 native-code 헌팅에서는 검증 환경이 특히 중요하며, 에이전트가 실패를 관찰하고 sanitizer 출력을 읽고 PoC를 고치거나 잘못된 후보를 버릴 수 있어야 합니다.

- [Security Cryptography Whatever — Nicholas Carlini와 AI Bug Finding](https://securitycryptographywhatever.com/2026/03/25/ai-bug-finding/)
  - AI 기반 버그 발견을 확장할 때 생기는 운영상 병목을 배울 수 있습니다. 재사용 가능한 하네스는 많은 후보를 빠르게 찾을 수 있지만, 검증, 책임 있는 제보, maintainer가 바로 행동할 수 있는 보고서 작성이 병목이 됩니다. 처리량을 높이려면 적극적인 중복 제거, 보고서 품질 관리, 심각한 후보에 대한 사람 검토 예산, 수정 비용 측정이 필요합니다.

- [Monad — Monad Bugfinder](https://www.monad.xyz/blog/monad-bugfinder)
  - Mythos와 TxRay에서 영감을 받은 Web3 작업 흐름을 배울 수 있습니다. 발견과 검증을 분리하고, 모든 실마리를 구조화된 데이터베이스에 저장하며, triage를 후보 생성과 별도 작업으로 취급합니다. 진지한 자율 헌팅에는 채팅 기록에 의존하는 방식이 아니라 가설, 증거, 중복 여부, 공격 시도, 검토자 판단을 담는 지속적인 기록원이 필요합니다.

- [Gustavo Grieco — Quimera 소개](https://gustavo-grieco.github.io/blog/introducing-quimera/)
  - Ethereum contract를 위한 피드백 기반 익스플로잇 생성을 배울 수 있습니다. 소스코드, 온체인 맥락, Foundry trace, Solidity PoC 반복을 “자금 탈취” 같은 단일 고영향 목표에 맞춰 결합합니다. 취약점 종류를 좁히고, 실행 가능한 성공 기준을 연결하고, trace 실패가 다음 전략을 이끌게 해야 막연한 감사 코멘트에 머물지 않습니다.

- [Yue Xue — AI Auditing Methodology: Agent Self-Evolution, Drift, Reverse Evolution and Solutions](https://www.linkedin.com/pulse/ai-auditing-methodology-agent-self-evolution-drift-reverse-yue-xue-uqsse/)
  - 장기 실행 감사 시스템이 어떻게 실패하는지 배울 수 있습니다. vulnerability drift는 에이전트를 더 쉬운 주변 문제로 끌고 가고, reverse evolution은 프롬프트만 길게 만들 뿐 적중률을 높이지 못합니다. 재사용할 설계는 분해 → 작업 정의 → 추론이며, 각 작업은 대상, 경계, 증거, 완료 조건, 검증자, 국소적 개선 규칙으로 고정되어야 합니다.

- [Yue Xue — AI Auditing Methodology, Part I](https://x.com/xy9301/status/2033186266649640980)
  - X 아티클의 핵심 주장을 배울 수 있습니다. AI 보안은 프레임워크 중심 파이프라인에서 코딩 에이전트 중심 흐름으로 이동하고 있으며, 먼저 인간의 감사 과정을 재현한 뒤 자동화하는 방향으로 바뀌고 있습니다. 진지한 헌터를 만들려면 모든 것을 무거운 정적 프레임워크로 전처리하기보다, 풍부한 프로젝트 자료 위에서 유연하게 탐색을 시작하는 편이 좋습니다.

- [Yue Xue — AI Auditing Methodology, Part II](https://x.com/xy9301/status/2036017855381340269)
  - 글에서 말하는 자기 개선의 조건인 variation, selection, inheritance를 배울 수 있습니다. 버그 헌팅 시스템에서는 새 프롬프트, 스킬, 분해 방식을 시도하고, benchmark나 사람 검토로 평가하며, 되돌릴 수 있는 경험을 유지해야 합니다. 중복 제거, 검증, PoC 생성은 부가 기능이 아니라 작업 흐름의 핵심 단계로 다뤄야 합니다.

- [pkqs91 / Octane Security — Codex로 3개월 만에 $200k를 번 방법](https://x.com/pkqs91/status/2070157806104457395)
  - X 아티클이 설명하는 실제 버그바운티 흐름을 배울 수 있습니다. intake 단계에서는 범위, 심각도 규칙, 제외 조건, 문서, 코드를 로컬 분석 묶음으로 바꾸고, hunting 단계에서는 넓게 탐색하며, verification 단계에서는 범위 밖 후보, 방어 로직 때문에 불가능한 경로, 영향이 없는 실마리를 제거합니다. 핵심 규칙은 “넓게 탐색하고, 깊게 악용 가능성을 검증하라”이며, 영향 필터를 통과한 소수만 수동 재현해야 합니다.

- [Plamen Tsanev — BEAST post](https://x.com/p_tsanev/status/2041880807032119670)
  - “몇 개의 에이전트가 전체 코드베이스를 읽게 하는” 첫 실험이 왜 실패했고, Plamen이 왜 전문화된 재귀 구조로 이동했는지 배울 수 있습니다. 글은 정찰, 초기 설정, 넓은 탐색, 인벤토리/의미 분석, 깊은 분석, 체인 분석, PoC, 보고서의 8단계 흐름을 제시합니다. 인간의 피로 때문에 놓치기 쉬운 복합 logic bug를 찾는 데 특히 유용합니다.

- [Zero Cool Labs — Symmetry Sniper](https://x.com/ZeroCool_AI/status/2069443859327705497)
  - X 아티클의 좁은 범위 전문 스킬 패턴을 배울 수 있습니다. 문서, 이름, 이벤트, 인터페이스, 테스트에서 의도된 대칭 관계를 먼저 확인한 뒤, 상태 변경, 자산 이동, 권한 검사, 반올림, 0 값, 경계값 처리를 비교합니다. 보고 가능한 취약점은 불변식 위반, 권한 불일치, 수수료/반올림 비대칭, batch 우회처럼 측정 가능한 영향만 허용하므로 잡음이 적은 스킬 설계에 좋습니다.

- [Alin-Mihai BARBATEI — Smart contract security auditing harness 구축 노트](https://www.linkedin.com/posts/alin-mihai-barbatei-27772b54_notes-on-building-a-smart-contract-security-activity-7476636721527455744-favn)
  - Web3 감사 하네스의 수명주기 분류를 배울 수 있습니다. 발견/정찰, 실마리 생성, 취약점 식별, 이슈 검증, PoC 작성으로 나누는 관점이 핵심입니다. 각 에이전트를 단계별로 배치하고, 모든 후보가 “흥미로운 실마리”에서 “검증된 공격 시나리오”로 이동했는지 확인하는 체크리스트로 유용합니다.

- [Night-Wolf — Reasoning-First 취약점 연구로 오픈소스 프로젝트에서 여러 버그 찾기](https://blogs.night-wolf.io/reasoning-first-vulnerability-research-that-found-multiples-bugs-in-open-source-project)
  - 글이 제시하는 명시적 루프를 배울 수 있습니다. MODEL로 대상을 모델링하고, HYPOTHESIZE로 공격자가 제어하는 입력과 깨지는 가정을 세우며, INVESTIGATE로 코드와 dataflow를 읽고, CONFIRM으로 최소 로컬 PoC를 만들고, RECORD하거나 버립니다. 에이전트가 추론에서 증거로 넘어가도록 강제하는 좋은 틀입니다.

- [Night-Wolf — Closed Source Software에서의 AI-Powered Bug Hunting](https://blogs.night-wolf.io/ai-powered-bug-hunting-in-closed-source-software)
  - 소스코드가 없을 때 에이전트 기반 헌팅을 바꾸는 방법을 배울 수 있습니다. 디컴파일하고, 디컴파일러용 스킬을 만들고, 여러 관점의 검토를 돌리고, 불가능한 전제조건을 걸러내며, 블랙박스 검증으로 후보를 확인합니다. 리버싱 산출물은 불완전한 증거이므로 실제 바이너리에서 주장한 공격 경로가 존재한다는 런타임 확인이 필요합니다.

- [Devansh — Needle in the Haystack: 취약점 연구를 위한 LLM](https://devansh.bearblog.dev/needle-in-the-haystack/)
  - 막연한 “버그 찾아줘”식 프롬프트가 왜 실패하는지 배울 수 있습니다. threat model, 공격자 능력, 신뢰 경계, 과거 CVE 맥락이 없으면 모델은 CWE 이름만 닮은 환각성 지적을 많이 만듭니다. 재사용할 기본 절차는 작게 시작하는 것입니다. 얇은 범위를 고르고, 한 페이지 threat model을 만들고, 불변식과 진입점을 찾은 뒤 악용 가능성을 증명하는 call chain과 테스트를 요청합니다.

- [Cykor — AI 기반 취약점 탐지 워크플로 만들기](https://blog.cykor.kr/2026/06/Building-an-AI-Based-Vulnerability-Detection-Workflow)
  - 실전에서 나온 세 단계 흐름을 배울 수 있습니다. 대상 전처리, 취약점 가설 생성, 후보 검증을 분리합니다. 미묘한 교훈은 정책 맥락입니다. 많은 취약점 후보가 회색지대에 있기 때문에, 에이전트는 코드 패턴을 곧바로 악용 가능하다고 부르기 전에 서비스 의도와 책임 경계를 알아야 합니다.

## 프로젝트

- [Anthropic — Defending Code Reference Harness](https://github.com/anthropics/defending-code-reference-harness)
  - C/C++ 메모리 버그를 위한 정찰 → 발견 → 검증 → 보고 → 패치 reference implementation을 배울 수 있습니다. Docker, ASAN, gVisor 격리, 병렬 에이전트가 포함됩니다. 재사용할 부분은 단계별 계약, sandbox 밖 실행 거부 규칙, 새 컨테이너 검증, 중복 제거, 악용 가능성 보고, 원래 PoC와 재탐색 모두에 대한 패치 검증입니다.

- [Anthropic Reference Harness — Claude Skills](https://github.com/anthropics/defending-code-reference-harness/tree/main/.claude/skills)
  - 보안 작업을 재사용 가능한 에이전트 스킬로 만드는 법을 배울 수 있습니다. quickstart, threat-model, vuln-scan, triage, patch, customize가 예시입니다. 전문가의 검토 습관을 즉흥적인 채팅 지시가 아니라 입력, 산출물, 안전 경계, 기대 출력이 정해진 반복 가능한 명령으로 바꾸는 틀로 유용합니다.

- [Cloudflare — security-audit-skill](https://github.com/cloudflare/security-audit-skill)
  - 간결한 6단계 코딩 에이전트 감사 스킬을 배울 수 있습니다. 병렬 정찰, 여러 관점의 탐색, 적대적 검증, 중복 제거, 누적 결과, 기계가 읽을 수 있는 출력이 포함됩니다. 가장 중요한 재사용 규칙은 검증 담당을 발견 담당과 분리하고, 후보가 살아남기 전에 악용 가능성을 적극적으로 반박하게 하는 것입니다.

- [Visa — Visa Vulnerability Agentic Harness](https://github.com/visa/visa-vulnerability-agentic-harness)
  - 특정 벤더에 묶이지 않는 SAST 하네스를 배울 수 있습니다. threat modeling, 여러 관점의 분석, 결정론적 투표, 적대적 검증, 구조화된 Markdown/SARIF 결과, remediation, validation이 포함됩니다. 기업 시스템에서는 AI가 발견한 악용 가능성부터 검증된 수정까지 전체 수명주기를 최적화한다는 점이 특히 유용합니다.

- [berabuddies — AgentFlow](https://github.com/berabuddies/agentflow)
  - 에이전트 기반 버그 헌팅 시스템을 typed graph로 표현하는 법을 배울 수 있습니다. 파일 또는 가설별 fanout, 결과 병합, 검토 실패에 대한 반복 루프, 여러 모델 실행, trace 기반 에이전트 개선이 가능합니다. 수천 개의 제한된 에이전트, 의존성 관계, 공유 작업 공간, 측정 가능한 종료 조건이 필요한 흐름의 구현 패턴으로 볼 만합니다.

- [Trail of Bits — Buttercup](https://github.com/trailofbits/buttercup)
  - OSS-Fuzz 스타일 대상을 찾고 패치하는 실행 가능한 AIxCC 기반 취약점 발견·패치 파이프라인을 배울 수 있습니다. 작업 모니터링, fuzzing, PoV 처리, 패치 생성이 핵심입니다. 전통적 동적 테스트와 LLM 에이전트가 상태를 공유하고, 중복 작업을 피하고, 단순 제안이 아니라 검증된 수정을 만드는 방식을 연구하기 좋습니다.

- [AISLE — nano-analyzer](https://github.com/weareaisle/nano-analyzer)
  - LLM 기반 native-code scanner의 가장 작은 유용한 버전을 배울 수 있습니다. 파일별 문맥 생성, 취약점 스캔, 회의적 triage, 최종 판정이 핵심입니다. 추가적인 에이전트 기능이 실제로 가치 있는지 비교할 수 있는 저비용 coverage-first 기준선으로 좋습니다.

- [PlamenTSV — plamen](https://github.com/PlamenTSV/plamen)
  - BEAST 아이디어의 실행 가능한 버전을 배울 수 있습니다. 18–100개의 Claude/Codex 에이전트가 8단계로 동작하며, 스마트 컨트랙트와 L1 노드 클라이언트 인프라에 대해 검증된 PoC 익스플로잇이 포함된 감사 보고서를 생성합니다. PTY로 감독되는 worker, 디스크 산출물을 기준 정보로 삼는 방식, 재개 가능한 checkpoint, PoC pass/fail 표시, 남은 의무를 드러내는 방식이 주요 구현 세부사항입니다.

## 논문

- [Synthesizing Multi-Agent Harnesses for Vulnerability Discovery](https://arxiv.org/abs/2604.20801)
  - 프롬프트뿐 아니라 하네스 자체를 최적화하는 방법을 배울 수 있습니다. 역할, 도구, 통신 구조, 재시도, 조율 규칙을 typed graph DSL로 모델링합니다. 핵심 교훈은 대상에서 나온 런타임 피드백으로 어떤 하네스 구성요소가 실패했는지 진단하고, 체계적으로 다시 설계할 수 있다는 점입니다. 논문은 알려지지 않았던 Chrome zero-day 발견도 보고합니다.

- [Knowdit — Auditing Knowledge Summarization을 이용한 Agentic Smart Contract Vulnerability Detection](https://arxiv.org/html/2603.26270v2)
  - 논문 기반 지식 주도 스마트 컨트랙트 감사를 배울 수 있습니다. 인간 감사 보고서에서 감사 지식 그래프를 만들고, DeFi 의미와 버그 패턴을 대상에 대응시킨 뒤, 명세 생성, PoC 합성, 실행, 회고를 반복합니다. 보고된 high 9개와 medium severity unknown finding 36개는 도메인 지식과 실행 가능한 증거가 일반적인 Solidity 냄새 탐지보다 강하다는 점을 보여줍니다.

- [AI Agent Smart Contract Exploit Generation](https://arxiv.org/html/2507.05558v2)
  - A1의 실행 중심 익스플로잇 모델을 배울 수 있습니다. LLM에 도메인 도구를 제공하고, 공격 전략을 생성하고, fork된 체인 상태에서 Solidity PoC를 실행하며, 실행 피드백으로 전략을 다듬습니다. 논문의 핵심 원칙은 “구체적으로 검증된 수익성 있는 PoC 익스플로잇만 보고한다”는 점이며, 62.96% VERITE 성공률은 Web3 에이전트 출력의 강한 기준으로 유용합니다.
