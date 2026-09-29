# 민병희

GPS · 걸음 수 같은 노이즈 섞인 신호나 병원 문서 같은 규칙에서 **판단 기준을 정하고, 그 기준이 실제로 맞는지 숫자로 확인하는** 일을 합니다. 애매하면 판정하지 않고 사람에게 넘기는 쪽을 택합니다.
무엇을 만들지와 판단 기준은 제가 정하고, 구현은 Claude Code와 함께 합니다.

**한눈에**

- **[러닝그라운드](https://github.com/Min-heee/running-ground)** — 여러 폰이 실시간으로 거리를 겨루는 러닝 앱, **출시 후 운영 중** · [App Store](https://apps.apple.com/kr/app/id6762328694) · [Google Play](https://play.google.com/store/apps/details?id=com.minheee.runnigapp) — 테스트 2,064개 · 화면 라우트 57개 · 사례연구 4편
- **[한창구](https://github.com/Min-heee/hanchanggu)** — 의원의 여러 문의 창구를 한 목록에 모으고, 병원 문서 근거가 붙은 답장 초안을 만드는 도구 + 같은 엔진의 사내 Q&A · [설계 문서](https://github.com/Min-heee/hanchanggu/blob/main/docs/PRD.md) — 테스트 432개
- **[다시봄](https://github.com/Min-heee/dasibom)** — 수술 · 시술 뒤 관리 일정을 규칙으로 계산해 "오늘 연락할 환자"를 뽑는 도구, AI 없이 결정적 계산 · [설계 문서](https://github.com/Min-heee/dasibom/blob/main/docs/PRD.md) — 테스트 352개
- **[같은각도](https://github.com/Min-heee/same-angle)** — 경과 사진을 기준 사진과 같은 촬영 조건으로 찍게 돕는 웹앱, 실기기 점검 단계 · [설계 문서](https://github.com/Min-heee/same-angle/blob/main/docs/PRD.md)
- **[모꼬지](https://github.com/Min-heee/mokkoji)** — 약속에 포인트를 걸고 늦으면 잃는 앱, 서버 연결 전 · [설계 문서](https://github.com/Min-heee/mokkoji/blob/main/docs/PRD.md) — 테스트 712개 · 화면 15개

---

### 러닝그라운드 · 러닝 앱 · 운영 중

[App Store](https://apps.apple.com/kr/app/id6762328694) · [Google Play](https://play.google.com/store/apps/details?id=com.minheee.runnigapp) · [코드](https://github.com/Min-heee/running-ground)

GPS로 러닝을 측정하고 여러 대의 폰이 같은 시각에 출발해 실시간으로 거리를 겨룹니다. 혼자 달리기, 1:1 · 그룹 대결, 파티런, 지역 랭킹, 월간 크루 리그가 있고, 자동차 · 자전거로 이동한 기록은 걸음 수와 보폭으로 걸러냅니다.
**커밋 1,461개**(1인, 2026-03-30~09-19) · **테스트 2,064개** · **화면 라우트 57개**. 저장소에 전체 코드와 커밋 이력, 아키텍처 문서, **사례연구 4편**이 있습니다 — 상대 거리가 90초 넘게 `0.00km`로 얼던 문제의 진범은 서버의 한 행이 3MB로 부푼 것이었고, 그중 **93%가 그 요청에서 아무도 읽지 않는 GPS 경로**였습니다. 떼어 내자 배포 직후 현장에서 204 kB가 됐습니다.
운영 중인 원본 저장소의 이력을 그대로 옮기고 회원 닉네임 · 실명과 키 · 서버 주소 등을 바꾼 공개 사본입니다.
`Expo/React Native` · `Node.js` · `PostgreSQL` · Swift · Kotlin 네이티브 모듈

### 가상 의원 도구 셋 · 한창구 · 다시봄 · 같은각도

모발 치료 의원을 가정해 만든 도구 셋입니다. **한창구**는 환자가 먼저 보낸 문의의 답장 초안, **다시봄**은 병원이 먼저 거는 연락, **같은각도**는 경과 사진 촬영을 맡습니다.
셋 다 가상 의원 '샘플의원'의 문서와 합성 데이터만 쓰고, 실제 병원 · 환자 정보는 쓰지 않았습니다. 병원 현장 검증은 아직 하지 않았고, 셋 모두 코드보다 설계 문서(PRD)를 먼저 커밋했습니다.

#### 한창구 · 문의함 + 사내 Q&A

[설계 문서](https://github.com/Min-heee/hanchanggu/blob/main/docs/PRD.md) · [코드](https://github.com/Min-heee/hanchanggu)

메신저 · 홈페이지 폼 · 예약 요청사항 · 리뷰 등으로 흩어진 문의를 직원 화면 하나에 모으고, 병원이 승인한 문서만 근거로 답장 초안을 만듭니다. 보내는 건 사람입니다. 직원의 내부 질문(사내 Q&A)에도 같은 문서와 같은 엔진을 씁니다.
증상 · 약 문의는 모델을 부르기 전에 **적신호 규칙 게이트**가 의료진 인계로 보냅니다. 초안은 Claude API의 문서 인용 기능으로 받고, 인용 원문과 문장 속 숫자 · 기간이 문서와 같은지 코드가 대조해 하나라도 어긋나면 초안 전체를 보류합니다. 금액은 모델이 쓰지 않고 가격표에서 채웁니다.
AI 초안을 두 번 녹화해 쟀습니다. 1회차에서 답할 수 있는 문항 15/39가 보류돼 지시문과 검증기를 고쳤고, 2회차는 **오보류 3/39 · 인용 원문 불일치 0/45**입니다. 같은 문항을 보고 고쳤으니 2회차에는 그 편향이 있고, 초안 품질 판정은 AI의 1차 판정이라 제가 아직 검토하지 않았습니다.
**합성 문의 41건 · 가상 문서 22편 · 테스트 432개**. 시연은 녹화된 응답만 보여 줘서 API 키 없이 돌아갑니다.
`Next.js` · `TypeScript` · Claude API(문서 인용) · 검색은 한국어 2-gram BM25

#### 다시봄 · 사후관리 연락

[설계 문서](https://github.com/Min-heee/dasibom/blob/main/docs/PRD.md) · [코드](https://github.com/Min-heee/dasibom)

수술 뒤 D+1 · D+7 · 4주 · 6개월 · 1년 경과 진료와 두피 주사 회차를 병원 문서의 규칙으로 계산해, 코디네이터에게 오늘 연락할 환자를 이유와 함께 보여 줍니다. 모델을 한 번도 부르지 않는 결정적 계산입니다.
휴진일(일요일 · 법정 공휴일)에 걸린 날은 옮기고 사유를 남기고, 문서가 정한 범위 안에 진료일이 없으면 날짜를 지어내지 않고 간호팀 확인으로 보냅니다. 연락 문구는 승인된 안내문에 날짜만 채웁니다.
**합성 환자 127명 · 기대값 대조 540/540 · 테스트 352개**. 기대값은 앱 엔진을 쓰지 않은 별도 계산이지만 이것도 AI가 썼고, 제가 달력으로 검수하기 전입니다. 이 저장소는 주제 선정까지 Claude Code에 맡겼고, 그 사실을 README에 적어 두었습니다.
`Next.js` · `TypeScript` — 판단은 순수 함수, 변이 검사로 테스트가 버그를 잡는지 확인

#### 같은각도 · 경과 사진 촬영 보조

[설계 문서](https://github.com/Min-heee/same-angle/blob/main/docs/PRD.md) · [코드](https://github.com/Min-heee/same-angle)

새 사진을 찍을 때 기준 사진과 고개 각도 · 거리 · 화면 내 위치가 같은지 숫자로 확인하는 웹앱입니다. 모발 상태는 판단하지 않고 촬영 조건만 비교합니다. 서버가 없고, 허용 목록 밖 네트워크 요청은 CSP로 막습니다.
**아직 실기기 점검 단계입니다.** 아이폰 사파리에서 카메라 · 얼굴 모델 · 센서가 실제로 어떻게 동작하는지 재는 점검 페이지까지 만들었고, 촬영 게이트 · 판정 · 비교 화면은 그 결과를 본 뒤에 만듭니다.
`Next.js` · `TypeScript` · `MediaPipe`(얼굴 랜드마크)

### 모꼬지 · 약속 지키기 앱 · 개발 중

[설계 문서](https://github.com/Min-heee/mokkoji/blob/main/docs/PRD.md) · [코드](https://github.com/Min-heee/mokkoji)

주최자가 [시작하기]를 누른 그 순간부터 서로의 위치가 보이고, 늦은 사람의 포인트는 제시간에 온 사람들이 나눠 갖습니다. 만난 뒤에는 모임 정산(차수별 · 항목별 · 다중 통화)으로 그대로 이어집니다.
**커밋 52개 · 테스트 712개 · 화면 15개**, 1~3단계(가짜 서버 → 실제 지도와 GPS 도착 판정 → Supabase 규칙)까지 `main` 병합. 실제 서버 연결만 남았습니다.
`Expo` · `TypeScript` · `Supabase` — 지각 판정과 정산 규칙은 순수 함수로 떼어 테스트합니다.

---

### AI와 일하는 방식

- **결정은 제가, 구현은 Claude Code와.** 러닝그라운드는 6월 이후 커밋의 86%(커밋 메시지의 공동 작성자 표기 기준), 모꼬지와 가상 의원 도구 셋은 커밋 전부를 함께 썼고, 왜 그렇게 정했는지는 커밋 메시지에 적어 둡니다. AI가 한 일과 제가 한 일은 각 저장소 README에 나눠 적습니다.
- **AI로 AI를 검증합니다.** 큰 변경은 검토 에이전트를 여러 개 동시에 돌려 반박하게 하고 재현된 지적만 반영합니다. 화면 꺼짐 기록 손실을 고칠 때는 검토자 30개가 결함 19건(최고 등급 2건)을 찾았습니다.
- **테스트가 버그를 정말 막는지 확인합니다.** 고친 코드를 일부러 되돌려 테스트가 실패하는지 봅니다. 되돌려도 통과하는 테스트는 다시 씁니다.
- **규칙이 AI보다 먼저입니다.** 병원 도구에서는 답하면 안 되는 문의를 모델보다 규칙이 먼저 잡고, 근거가 어긋나면 초안을 보류하고, 보내는 건 언제나 사람입니다.
- **마지막은 실기기입니다.** 두 폰을 나란히 들고 뛰고, 부정행위 판정은 직접 자전거를 타서 확인합니다.

### 주로 쓰는 것

| | |
|---|---|
| **앱** | React Native · Expo · TypeScript · Expo Router |
| **웹** | Next.js · TypeScript · Vitest · MediaPipe |
| **백엔드 · 인프라** | Node.js · PostgreSQL · Docker · Caddy · DigitalOcean · Cloudflare · EAS Build/Update |
| **네이티브 · 3D** | Swift(iOS) · Kotlin(Android) 백그라운드 위치 모듈 · three.js + expo-gl |
| **AI** | Claude API(문서 인용) · Claude Code(git worktree 병렬 세션, 병렬 에이전트 검증) · OpenAI Codex(코드 감사) |

<sub>위 커밋 수와 테스트 수는 저장소에서 직접 센 값입니다. 가상 의원 도구의 수치는 합성 데이터 기준입니다.</sub>
