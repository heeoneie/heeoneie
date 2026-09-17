## 채희원 · heeoneie

리뷰 데이터에서 문제를 찾습니다.

| | 리뷰 데이터로 | 누구의 문제를 |
|---|---|---|
| **analyze-review** | 무엇부터 고쳐야 하는지 진단하고 답글을 생성 | 셀러의 운영 |
| **review-radar** | 가짜 리뷰를 판별해 신뢰도 등급을 표시 | 소비자의 구매 판단 |
| **OntoReview** | 소송이 되기 전에 법적 리스크를 탐지 | 기업의 법무 |

---

### OntoReview

**미국 식품·CPG 브랜드를 위한 소송 예방 시스템.** 고객 리뷰와 FDA 리콜 데이터에서 제조물 책임·집단소송 신호를 법적 절차가 시작되기 전에 잡아냅니다.

🔗 **[ontoreview.com](https://www.ontoreview.com)** · 운영 중

LLM 리스크 분류기와 OWL 2 온톨로지 추론을 openFDA 실데이터 위에서 돌립니다. 손해배상 추정액은 LLM이 아니라 규칙 기반 추정기가 계산하고, 어떤 openFDA 필드가 어떤 배수를 만들었는지 UI에 함께 표시합니다. 입력이 부족하면 숫자를 지어내지 않고 가립니다.

**백테스트 — 2023년 Victor Hi-Pro Plus 살모넬라 리콜**

| | |
|---|---|
| 1,559건 | 리콜 발표 이전 Chewy 리뷰 **전량** 판독 (샘플링 없음) |
| 9.6% → 24% | 위험 언어 비중, 최고 70% |
| p = 0.027 | 단측 2비율 z-검정 · 49개 롤링 윈도우 중 28개 유의 |
| **42주** | 첫 지속 신호 → 리콜 발표 |
| $5.5M | 뒤따른 집단소송 합의금 |

상관이지 인과가 아니며, 하나의 사례는 하나의 사례일 뿐입니다. 이 한계는 제품 랜딩에도 그대로 적어두었습니다.

`Python` `FastAPI` `owlready2` `React` `PostgreSQL` `WorkOS` `Celery` `Docker` `Cloudflare Tunnel`

---

### 그 외 작업

**[analyze-review](https://github.com/heeoneie/analyze-review)** — 이커머스 통합 솔루션이 다루지 않던 리뷰 카테고리를 만드는 데서 출발했습니다. 셀러용 분석기를 만든 뒤 실제 배달 매장에 도입하면서, 문제가 예상과 다르다는 걸 리뷰 441건을 직접 세어보고 알았습니다. 불만 처리가 아니라 *사과할 일이 없어서 인사말이 반복되는 것* 이 진짜 문제였습니다. 답글 생성기는 현재 실매장에서 운영 중입니다.

**[review-radar](https://github.com/heeoneie/review-radar)** — 같은 기술을 소비자 쪽으로 돌린 크롬 확장. 별점 분포·구매 인증 비율·리뷰 폭주·코사인 유사도 클러스터 네 신호를 로컬에서 계산하고 AI 판단과 40:60으로 합산합니다.

---

### 오픈소스 기여

**[apache/maka](https://github.com/apache/maka)** — Apache Software Foundation (incubating)

- 샌드박스 파일시스템 워커의 Grep이 `-`로 시작하는 패턴을 ripgrep 플래그로 넘기고 있었습니다. 파일에 있는 문자열을 오류 없이 "없음"으로 보고하던 버그라, 같은 저장소가 이미 올바르게 처리하던 방식으로 맞추고 회귀 테스트를 붙였습니다. — [#3903](https://github.com/apache/maka/pull/3903) merged
- Agent Graph 라벨이 flex 수축으로 세로로 쪼개지던 문제. 중국어 전용으로 보고됐지만 영어도 기본 폭에서 깨진다는 걸 확인해, "라벨은 수축하지 않는다"로 범위를 다시 잡았습니다. — [#3970](https://github.com/apache/maka/pull/3970) merged
- 더 이상 호출되지 않는 레거시 Usage 세션 스캐너를 스토리지 경로에서 제거했습니다. 전용 테스트와 entrypoint 기대값까지 함께 지우고, 이슈가 범위 밖으로 지정한 것은 건드리지 않았습니다. — [#4060](https://github.com/apache/maka/pull/4060) merged

한국어 로케일([#3975](https://github.com/apache/maka/issues/3975))을 맡아 진행 중입니다. 데스크톱 슬라이스 1의 문자열 1,006개를 다섯 카탈로그에 넣고 언어 선택기를 열었습니다([#5451](https://github.com/apache/maka/pull/5451)). 번역보다 용어집을 먼저 고정했고, 용어 빈도는 추정 대신 스크립트로 재서 다시 셀 수 있게 남겼습니다.

작업 중 세 가지를 따로 이슈로 냈습니다. `ko`를 넣어도 타입이 통과하면서 영어가 그대로 나가는 경로([#5075](https://github.com/apache/maka/issues/5075)), `workHubHelp`가 세 로케일에서 갈린 머지 순서 문제([#5289](https://github.com/apache/maka/issues/5289)), `zh-CN`과 `zh-TW`가 상수를 공유해 번체 사용자가 커맨드 팔레트에서 간체를 보게 되는 문제([#5377](https://github.com/apache/maka/issues/5377)).

### fedify — ActivityPub 프레임워크

- `PostgresMessageQueue`의 `initialized` 옵션이 스키마 DDL과 함께 JSON 직렬화 탐지까지 건너뛰어, 메시지가 JSONB 객체가 아니라 문자열로 저장되던 버그. 플래그를 둘로 쪼개 DDL만 막도록 했습니다. — [#1032](https://github.com/fedify-dev/fedify/pull/1032) merged
- 같은 결함이 `PostgresKvStore`에도 있었습니다. 큐의 잘못된 행은 디큐되며 스스로 사라지지만 KV의 행은 테이블에 남아, `initialized`를 준 적 없는 프로세스의 읽기까지 문자열로 돌려줍니다. 이슈로 먼저 내고([#1031](https://github.com/fedify-dev/fedify/issues/1031)) 고쳤습니다. — [#1033](https://github.com/fedify-dev/fedify/pull/1033) merged

`RedisKvStore`가 소수 초 TTL을 그대로 `SETEX`에 넘겨 쓰기가 실패하던 버그([#1034](https://github.com/fedify-dev/fedify/pull/1034))와, 공개키·서명 스펙 캐시에 수명 상한을 넣는 작업([#1027](https://github.com/fedify-dev/fedify/pull/1027))이 리뷰 중입니다. 그 과정에서 `@fedify/fixture`로 등록한 테스트 229개 중 228개가 Node.js에서 실행되지 않은 채 통과로 집계되고 있다는 것을 찾아 이슈로 냈습니다([#1036](https://github.com/fedify-dev/fedify/issues/1036)). ESM에서 `require`가 던지고 빈 `catch`가 삼키는 구조였습니다.

### data-privacy-stack/presidio

- 한국어 인식기들이 `supported_languages`에 ISO 639-1이 아닌 `kr`을 `ko`와 함께 싣고 있었고, `UkPostcodeRecognizer` 항목이 중복돼 있었습니다. 이미 `kr`로 설정된 레지스트리가 깨지지 않도록 두 인식기에만 하위 호환 별칭으로 남기고 나머지를 정리했습니다. — [#2236](https://github.com/data-privacy-stack/presidio/pull/2236) merged

---

[![linkedin](https://img.shields.io/badge/LinkedIn-heeoneie-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/heeoneie)
