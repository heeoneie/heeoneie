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
| 1,559건 | 2026-07-14 수집분 2,429건 중 리콜 이전 리뷰 판독 |
| 9.6% → 24% | 분류기가 위험으로 판정한 리뷰 비중, 최고 70% |
| p = 0.027 | 단측 2비율 z-검정 · 49개 롤링 윈도우 중 28개 유의 |
| **42주** | 첫 지속 신호 → 리콜 발표 |
| $5.5M | 뒤따른 집단소송 합의금 |

상관이지 인과가 아니며, 하나의 사례는 하나의 사례일 뿐입니다. 이 한계는 제품 랜딩에도 그대로 적어두었습니다.

`Python` `FastAPI` `owlready2` `React` `PostgreSQL` `WorkOS` `Celery` `Docker` `Cloudflare Tunnel`

---

### 그 외 작업

**[analyze-review](https://github.com/heeoneie/analyze-review)** — 이커머스 통합 솔루션이 다루지 않던 리뷰 카테고리를 만드는 데서 출발했습니다. 셀러용 분석기를 만든 뒤 친구의 배달 매장 도입을 준비하면서, 문제가 예상과 다르다는 걸 그 매장 리뷰 441건을 직접 세어보고 알았습니다. 불만 처리가 아니라 *사과할 일이 없어서 인사말이 반복되는 것* 이 진짜 문제였습니다. 답글 생성기를 마무리하는 중이며, 완성 후 해당 매장에 시범 도입할 예정입니다.

**[review-radar](https://github.com/heeoneie/review-radar)** — 같은 기술을 소비자 쪽으로 돌린 크롬 확장. 별점 분포·구매 인증 비율·리뷰 폭주·코사인 유사도 클러스터 네 신호를 로컬에서 계산하고 AI 판단과 40:60으로 합산합니다.

---

### 오픈소스 기여

머지 9건 — apache/maka 3 · fedify 4 · lindera 1 · presidio 1

**[apache/maka](https://github.com/apache/maka)** — Apache Software Foundation (incubating)

- 샌드박스 파일시스템 워커의 Grep이 `-`로 시작하는 패턴을 ripgrep 플래그로 넘기고 있었습니다. 파일에 있는 문자열을 오류 없이 "없음"으로 보고하던 버그라, 같은 저장소가 이미 올바르게 처리하던 방식으로 맞추고 회귀 테스트를 붙였습니다. — [#3903](https://github.com/apache/maka/pull/3903) merged
- Agent Graph 라벨이 flex 수축으로 세로로 쪼개지던 문제. 중국어 전용으로 보고됐지만 영어도 기본 폭에서 깨진다는 걸 확인해, "라벨은 수축하지 않는다"로 범위를 다시 잡았습니다. — [#3970](https://github.com/apache/maka/pull/3970) merged
- 더 이상 호출되지 않는 레거시 Usage 세션 스캐너를 스토리지 경로에서 제거했습니다. 전용 테스트와 entrypoint 기대값까지 함께 지우고, 이슈가 범위 밖으로 지정한 것은 건드리지 않았습니다. — [#4060](https://github.com/apache/maka/pull/4060) merged

한국어 로케일([#3975](https://github.com/apache/maka/issues/3975))을 맡아 진행 중입니다. 로케일 모델을 여는 코어([#5011](https://github.com/apache/maka/pull/5011))는 메인테이너 리뷰를 통과했고, 공식 통합 스택([#5100](https://github.com/apache/maka/pull/5100))에 공동 저자로 옮겨졌습니다. 데스크톱 슬라이스 1([#5451](https://github.com/apache/maka/pull/5451))에서는 문자열 1,006개를 다섯 카탈로그에 넣고 언어 선택기를 열었습니다. 조사·어미 규칙과 영어 상수로 새는 경로를 막는 테스트 11개도 함께 넣었습니다. 번역보다 용어집을 먼저 고정했고, 용어 빈도는 추정 대신 스크립트로 재서 다시 셀 수 있게 남겼습니다. 나머지 슬라이스가 모이면 한 번에 통합됩니다.

작업 중 세 가지를 따로 이슈로 냈습니다. `ko`를 넣어도 타입이 통과하면서 영어가 그대로 나가는 경로([#5075](https://github.com/apache/maka/issues/5075)), `workHubHelp`가 세 로케일에서 갈린 머지 순서 문제([#5289](https://github.com/apache/maka/issues/5289)), `zh-CN`과 `zh-TW`가 상수를 공유해 번체 사용자가 커맨드 팔레트에서 간체를 보게 되는 문제([#5377](https://github.com/apache/maka/issues/5377)).

**[fedify](https://github.com/fedify-dev/fedify)** — ActivityPub 프레임워크

- `PostgresMessageQueue`의 `initialized` 옵션이 스키마 DDL과 함께 JSON 직렬화 탐지까지 건너뛰어, 메시지가 JSONB 객체가 아니라 문자열로 저장되던 버그. 플래그를 둘로 쪼개 DDL만 막도록 했습니다. — [#1032](https://github.com/fedify-dev/fedify/pull/1032) merged
- 같은 결함이 `PostgresKvStore`에도 있었습니다. 큐의 잘못된 행은 디큐되며 스스로 사라지지만 KV의 행은 테이블에 남아, `initialized`를 준 적 없는 프로세스의 읽기까지 문자열로 돌려줍니다. 이슈로 먼저 내고([#1031](https://github.com/fedify-dev/fedify/issues/1031)) 고쳤습니다. — [#1033](https://github.com/fedify-dev/fedify/pull/1033) merged
- 공개키·HTTP 서명 스펙 캐시가 만료 없이 쌓여, 한 인스턴스의 Redis에만 19만 건이 넘게 남아 있었습니다. 구현 전에 두 가지 설계를 비교해 제안했고, 메인테이너와 합의한 형태로 쓰기 시점에 수명(키 30일, 스펙 90일, 설정 가능)을 넣고 기존 항목은 운영 문서로 정리하도록 했습니다. — [#1027](https://github.com/fedify-dev/fedify/pull/1027) merged
- `RedisKvStore`가 소수 초 TTL을 그대로 `SETEX`에 넘겨 쓰기가 실패하던 버그. 다른 KV 어댑터와 비교해 호출자가 요청한 수명보다 짧아지지 않도록 올림으로 맞췄고, 통합 테스트는 실행 속도에 흔들리지 않도록 경과 시간을 감안한 `PTTL` 범위로 검증합니다. — [#1034](https://github.com/fedify-dev/fedify/pull/1034) merged

그 과정에서 `@fedify/fixture`로 등록한 테스트 229개 중 228개가 Node.js에서 실행되지 않은 채 통과로 집계되고 있다는 것을 찾아 이슈로 냈습니다([#1036](https://github.com/fedify-dev/fedify/issues/1036)). ESM에서 `require`가 던지고 빈 `catch`가 삼키는 구조였고, 메인테이너가 고친 뒤([#1039](https://github.com/fedify-dev/fedify/pull/1039)) `@fedify/redis`의 Node.js 테스트는 7개에서 14개로 늘었습니다.

**[lindera](https://github.com/lindera/lindera)** — Rust 형태소 분석 라이브러리

- 일본어에는 한자 숫자를 아라비아 숫자로 바꾸는 토큰 필터가 있는데 한국어에는 없었습니다(2022년부터 열려 있던 이슈). 한자어 수사는 일본어와 자릿수 구조가 같아 알고리즘을 그대로 따르되, 고유어 수사는 자릿수 구조가 없어 변환하지 않습니다. 리뷰에서 그 경계가 제 생각보다 좁다는 걸 배웠습니다. `일곱`은 한자어 `일`로 시작해 글자 단위로 바꾸면 `1곱`이 되고, `하나`·`스물`은 수사 음절이 없어 우연히 무사했을 뿐이었습니다. Lucene의 `KoreanNumberFilter`처럼 **모든 글자가 수사일 때만 변환**하도록 게이트를 넣어 해결했습니다. — [#1024](https://github.com/lindera/lindera/pull/1024) merged · 6.1.0
- 처음 올린 본문에는 `이천이십육` → `2026`을 파이프라인 결과인 것처럼 적었는데, ko-dic은 이를 형태소 단위로 쪼개기 때문에 실제로는 `2 1000 2 10 6`이 나옵니다. 메인테이너가 실제 출력으로 지적해 주었고, 손으로 만든 토큰 대신 사전을 실제로 로드하는 테스트로 바꾸고 문서에 이 사실을 명시했습니다. 토큰 병합은 메인테이너가 별도 이슈로 진행 중이며, 그 뒤 일본어·한국어 필터를 합치는 공용 엔진을 맡기로 했습니다.

**[data-privacy-stack/presidio](https://github.com/data-privacy-stack/presidio)** — PII 탐지·비식별화

- 한국어 인식기들이 `supported_languages`에 ISO 639-1이 아닌 `kr`을 `ko`와 함께 싣고 있었고, `UkPostcodeRecognizer` 항목이 중복돼 있었습니다. 이미 `kr`로 설정된 레지스트리가 깨지지 않도록 두 인식기에만 하위 호환 별칭으로 남기고 나머지를 정리했습니다. — [#2236](https://github.com/data-privacy-stack/presidio/pull/2236) merged

**[Kiwi](https://github.com/bab2min/Kiwi) · [kiwipiepy](https://github.com/bab2min/kiwipiepy)** — 한국어 형태소 분석기

- 형태소를 다시 문장으로 잇는 `join`이 `( mike`, `9 %씩`처럼 띄어쓰기를 깨뜨린다는 제보. 원인은 규칙 순서였습니다. 괄호 예외가 "영문·숫자 앞에는 공백" 규칙보다 아래에 있어 `(`+`가`는 붙고 `(`+`mike`만 떨어졌고, 기호는 앞에만 공백을 넣어 `3`,`+`,`4`가 `3 +4`라는 어느 표기법도 아닌 결과가 됐습니다. — [Kiwi#278](https://github.com/bab2min/Kiwi/pull/278)
- 같은 제보의 `1인` → `1 인`은 고치지 않았습니다. 숫자 뒤 단위명사는 의존명사로 태깅되면 이미 붙고(`3등`, `10위`), 안 붙는 건 일반명사로 태깅되는 쪽(`인`, `층`, `급`)뿐이라 규칙이 아니라 분석 결과의 문제로 보였기 때문입니다. 메인테이너가 이 진단이 맞다고 확인했고, 후보 순위와 점수 차를 재서 "모델 튜닝으로 고칠 수 있는 것"과 "사전에 후보 자체가 없는 것"을 나눠 정리했습니다. — [kiwipiepy#220](https://github.com/bab2min/kiwipiepy/issues/220)
- 선어말어미를 어간에서 떼지 않는 옵션(`들어가/VV`+`시/EP` → `들어가시/VV`). 메인테이너가 구현 위치를 지정해 둔 이슈였습니다. — [Kiwi#279](https://github.com/bab2min/Kiwi/pull/279)
- 분석기 객체가 pickle되지 않아 멀티프로세싱 워커로 넘길 수 없던 문제. 사전 조작을 재생하는 방식으로 구현했으나, 메인테이너가 "복원할 수 없는 것을 절반만 복원하느니 명시적으로 실패하는 편이 낫다"며 더 좁은 설계를 제안해 그 방향으로 다시 쓰는 중입니다. — [kiwipiepy#235](https://github.com/bab2min/kiwipiepy/pull/235)

**[Haystack](https://github.com/deepset-ai/haystack)** — LLM 오케스트레이션 프레임워크

- 기본 BM25 토크나이저가 한국어 조사를 명사에서 떼어내지 못해, `서울`로 검색하면 `서울은`이 든 문서가 하나도 안 잡힙니다. 한국어 문장에서 명사는 거의 항상 조사가 붙은 채 나오므로 사실상 전량 실패입니다. 일화 대신 같은 구조의 한국어/영어 10개 주제 벤치마크로 재현했습니다(한국어 0/10, 영어 10/10). — [#12836](https://github.com/deepset-ai/haystack/issues/12836)
- 수정 PR이 올라오자 메인테이너가 원어민 검증을 요청했습니다. recall은 0/10 → 10/10으로 고쳐지지만 글자 단위 토큰은 정밀도를 잃습니다. `서울` 검색에 "서점 앞에서 울고 있는" 문서가 1위로 올라옵니다. 한글 한 글자는 단어보다 알파벳 한 글자에 가깝기 때문입니다. 유니그램·바이그램·둘 다를 같은 코퍼스에서 비교해 수치로 제출했고, NFD 정규화와 한국 한자가 쓰는 호환 한자 블록 누락도 함께 보고해 반영됐습니다. 남은 정밀도 문제는 메인테이너 요청으로 별도 이슈를 열어 설계를 논의 중입니다. — [#12867](https://github.com/deepset-ai/haystack/issues/12867)

**[Hackers' Pub](https://github.com/hackers-pub/hackerspub)** — fedify 기반 소셜 네트워크

- 한두 글자 한국어 검색이 느린 문제에서, 원인이 인덱스만이 아니라 `ANALYZE` 샘플에 따라 흔들리는 플래너 추정이라는 걸 측정으로 보였습니다. 같은 쿼리가 샘플에 따라 26ms도, 1.4초도 됩니다. 메인테이너 요청으로 `pg_trgm`·n-gram 배열·`pg_bigm`·ParadeDB `pg_search`를 같은 조건에서 비교했고, 결과가 기존 검색과 같은지는 차분 테스트 432건으로 확인했습니다. 최악 6초이던 쿼리가 세 후보 모두에서 30ms대로 내려옵니다. — [#384](https://github.com/hackers-pub/hackerspub/issues/384) 조사

---

[![linkedin](https://img.shields.io/badge/LinkedIn-heeoneie-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/heeoneie)
