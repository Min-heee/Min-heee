# 민병희

노이즈 섞인 센서 신호(위치 · 걸음 수 · 보폭 · 관절 좌표)로 **사람의 동작을 판정하는 규칙**을 정하고, 그 규칙이 실제 폰에서 맞는지 확인하는 일을 합니다.
무엇을 만들지와 판정 기준은 제가 정하고, 구현은 Claude Code와 함께 합니다.

**한눈에**

- **[품새 판정기](https://github.com/Min-heee/poomsae-judge)** — 웹캠·녹화 영상에서 관절 33개를 읽어 태권도 기본 동작을 공개된 규칙으로 채점하는 `Next.js` 웹 데모 · [바로 열기](https://poomsae-judge.vercel.app) — 판정 규칙 19개 · 테스트 303개
- **[러닝그라운드](https://github.com/Min-heee/running-ground)** — 여러 폰이 실시간으로 거리를 겨루는 러닝 앱, **출시 후 운영 중** · [App Store](https://apps.apple.com/kr/app/id6762328694) · [Google Play](https://play.google.com/store/apps/details?id=com.minheee.runnigapp) — 테스트 2,064개 · 화면 라우트 57개 · 사례연구 4편
- **[모꼬지](https://github.com/Min-heee/mokkoji)** — 약속에 포인트를 걸고 늦으면 잃는 앱, 서버 연결 전 · [설계 문서](https://github.com/Min-heee/mokkoji/blob/main/docs/PRD.md) — 테스트 712개 · 화면 15개

---

### 품새 판정기 · 웹 데모

[바로 열어 보기](https://poomsae-judge.vercel.app) · [코드](https://github.com/Min-heee/poomsae-judge)

<img src="https://raw.githubusercontent.com/Min-heee/poomsae-judge/main/docs/screenshots/01-judging.png" alt="앞차기를 채점한 화면. 점수 옆에 감점 항목과 측정하지 않은 것이 함께 있다" width="640">

<sub>샘플 재생 판정 화면 — 점수 옆에 감점 항목과 측정하지 않은 것이 같이 있습니다.</sub>

웹캠이나 녹화된 관절 좌표로 주춤서기 · 앞차기를 채점합니다. 점수 옆에 언제나 측정값과 경계값이 같이 있고, 신뢰도가 낮거나 촬영 각도가 규칙의 전제와 다르면 점수를 만들지 않고 **판정 보류**로 떨어뜨립니다. 카메라 권한 없이 합성 샘플로 전부 확인할 수 있습니다.
**감점 12개 · 보류 7개 = 판정 규칙 19개**, 샘플 7개 중 2개는 일부러 보류로 떨어뜨립니다. 판정 코어는 순수 함수로 분리해 **테스트 303개**로 고정했고, 기획서를 먼저 커밋하고 이틀(2026-09-22~23, 커밋 18개) 만에 여기까지 만들었습니다.
`Next.js` · `TypeScript` · `MediaPipe`(관절 33개) · `three.js`

### 러닝그라운드 · 러닝 앱 · 운영 중

[App Store](https://apps.apple.com/kr/app/id6762328694) · [Google Play](https://play.google.com/store/apps/details?id=com.minheee.runnigapp) · [코드](https://github.com/Min-heee/running-ground)

GPS로 러닝을 측정하고 여러 대의 폰이 같은 시각에 출발해 실시간으로 거리를 겨룹니다. 혼자 달리기, 1:1 · 그룹 대결, 파티런, 지역 랭킹, 월간 크루 리그가 있고, 자동차 · 자전거로 이동한 기록은 걸음 수와 보폭으로 걸러냅니다.
**커밋 1,461개**(1인, 2026-03-30~09-19) · **테스트 2,064개** · **화면 라우트 57개**. 저장소에 전체 코드와 커밋 이력, 아키텍처 문서, **사례연구 4편**이 있습니다 — 상대 거리가 90초 넘게 `0.00km`로 얼던 문제의 진범은 서버의 한 행이 3MB로 부푼 것이었고, 그중 **93%가 그 요청에서 아무도 읽지 않는 GPS 경로**였습니다. 떼어 내자 배포 직후 현장에서 204 kB가 됐습니다.
운영 중인 원본 저장소의 이력을 그대로 옮기고 회원 닉네임 · 실명과 키 · 서버 주소 등을 바꾼 공개 사본입니다.
`Expo/React Native` · `Node.js` · `PostgreSQL` · Swift · Kotlin 네이티브 모듈

### 모꼬지 · 약속 지키기 앱 · 개발 중

[설계 문서](https://github.com/Min-heee/mokkoji/blob/main/docs/PRD.md) · [코드](https://github.com/Min-heee/mokkoji)

주최자가 [시작하기]를 누른 그 순간부터 서로의 위치가 보이고, 늦은 사람의 포인트는 제시간에 온 사람들이 나눠 갖습니다. 만난 뒤에는 모임 정산(차수별 · 항목별 · 다중 통화)으로 그대로 이어집니다.
**커밋 52개 · 테스트 712개 · 화면 15개**, 1~3단계(가짜 서버 → 실제 지도와 GPS 도착 판정 → Supabase 규칙)까지 `main` 병합. 실제 서버 연결만 남았습니다.
`Expo` · `TypeScript` · `Supabase` — 지각 판정과 정산 규칙은 순수 함수로 떼어 테스트합니다.

---

### AI와 일하는 방식

- **결정은 제가, 구현은 Claude Code와.** 러닝그라운드는 6월 이후 커밋의 86%(커밋 메시지의 공동 작성자 표기 기준), 모꼬지는 커밋 전부를 함께 썼고, 왜 그렇게 정했는지는 커밋 메시지에 적어 둡니다.
- **AI로 AI를 검증합니다.** 큰 변경은 검토 에이전트를 여러 개 동시에 돌려 반박하게 하고 재현된 지적만 반영합니다. 화면 꺼짐 기록 손실을 고칠 때는 검토자 30개가 결함 19건(최고 등급 2건)을 찾았습니다.
- **테스트가 버그를 정말 막는지 확인합니다.** 고친 코드를 일부러 되돌려 테스트가 실패하는지 봅니다. 되돌려도 통과하는 테스트는 다시 씁니다.
- **마지막은 실기기입니다.** 두 폰을 나란히 들고 뛰고, 부정행위 판정은 직접 자전거를 타서 확인합니다.

### 주로 쓰는 것

| | |
|---|---|
| **앱** | React Native · Expo · TypeScript · Expo Router |
| **백엔드 · 인프라** | Node.js · PostgreSQL · Docker · Caddy · DigitalOcean · Cloudflare · EAS Build/Update |
| **네이티브 · 3D** | Swift(iOS) · Kotlin(Android) 백그라운드 위치 모듈 · three.js + expo-gl |
| **AI 도구** | Claude Code(git worktree 병렬 세션, 병렬 에이전트 검증) · OpenAI Codex(코드 감사) |

<sub>위 커밋 수와 테스트 수는 저장소에서 직접 센 값입니다.</sub>
