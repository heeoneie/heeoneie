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

**[review-radar](https://github.com/heeoneie/review-radar)** — 같은 기술을 소비자 쪽으로 돌린 크롬 확장. 별점 분포·구매 인증 비율·리뷰 폭주·코사인 유사도 클러스터 네 신호를 로컬에서 계산하고 AI 판단과 40:60으로 합산합니다. 아마존이 스크래핑을 막아서 방식을 네 번 갈아엎었습니다.

**[VOIX](https://github.com/heeoneie/VOIX)** — 음성으로 답하면 AI가 맥락을 읽고 다음 질문을 만드는 설문 시스템. `Next.js` `Gemini` `ElevenLabs` `Supabase`

---

### 오픈소스 기여

**[apache/maka](https://github.com/apache/maka)** (Apache Software Foundation, incubating) — 샌드박스 파일시스템 워커의 Grep이 `-`로 시작하는 패턴을 ripgrep 플래그로 넘기고 있었습니다. 파일에 있는 문자열을 오류 없이 "없음"으로 보고하던 버그라, 같은 저장소가 이미 올바르게 처리하던 방식으로 맞추고 회귀 테스트를 붙였습니다. — [#3903](https://github.com/apache/maka/pull/3903) merged

### 일하는 방식

혼자 만들 때도 PR로 나눠 올리고 코드리뷰를 붙입니다. 운영 중인 저장소에는 CI·린트·pre-commit·테스트를 걸어둡니다.

지난 1년 기여 **1,093회** · 팀 저장소 PR **105건** · `Pull Shark ×3` `Pair Extraordinaire ×4`

[![linkedin](https://img.shields.io/badge/LinkedIn-heeoneie-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/heeoneie)
