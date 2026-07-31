# [Project A] 작업 로그 — AI 기반 UI/UX 디자인 시안 제작

> **제출물 구분**: 결과물 ② 작업 로그 문서 (1개)
> **프로젝트명**: Project A — 러닝 크루 매칭 앱 화면 시안 제작
> **저장소**: `6minho/codyssey2-1` (branch: main)

이 문서는 `기획 → 프롬프트 → 이미지 생성 → 후가공 → 프로토타입`으로 이어지는
AI 협업 워크플로우의 **과정 기록**이다. 평가 대상은 결과 이미지의 완성도가 아니라
"어떤 단어를 넣었더니 무엇이 어떻게 달라졌는가"이므로, 화면별로 프롬프트 원문(v1 → v2)과
변경 사유·결과 차이를 그대로 남긴다.

---

## 0. 사용 도구 명시 (§6 필수 항목)

| 구분 | 도구 | 용도 |
|---|---|---|
| 이미지 생성 (최종 채택) | **ChatGPT — GPT Image 2 (gpt-image-2)** | 최종 3장 전체 생성 |
| 이미지 생성 (탐색·미채택) | Stable Diffusion 1.5 (로컬, AUTOMATIC1111), SD 1.5 + UI-UX LoRA, Ideogram, Bing Image Creator / Leonardo AI | 모델 비교·품질 확인 |
| 프로토타입 | **Figma** | 이미지 배치 및 Hotspot 화면 전환 연결 (링크 별도 관리) |

> 레퍼런스 출처(§6 필수): 외부 레퍼런스 이미지를 사용한 경우 이 표 아래에 출처(작성자·URL)를
> 기록해야 한다. 타인 작업물(Pinterest·Dribbble) 캡처 제출은 금지 사항이다.
> *→ 현재 저장소 문서에는 외부 레퍼런스 출처가 기재되어 있지 않음. 사용했다면 여기에 명시할 것.*

---

## 1. 워크플로우 개요

```
[기획]  서비스 컨셉·화면 3역할 정의 (메인 / 목록 / 상세)
   │
[모델 탐색]  SD 1.5 → LoRA → Ideogram → Bing/Leonardo → GPT Image 2 (최종)
   │
[프롬프트 최적화]  화면별 v1(초안) → v2(최종), 사유·결과 차이 기록
   │
[일관성 확보]  시드 미지원 → '스타일 고정 문장' + 원본 PNG 보존
   │
[후가공]  텍스트 깨짐·부자연 요소 대응
   │
[프로토타입]  Figma에 3장 배치 + 투명 Hotspot으로 화면 전환 연결
```

화면 흐름: `[메인] ─카드 클릭→ [목록] ─항목 클릭→ [상세] ─뒤로가기→ [메인]`

---

## 2. 모델 선택 이력 — 초안 단계의 탐색 기록

프롬프트 최적화에 앞서, 어떤 모델이 목적(한글 UI 화면 재현)에 맞는지 탐색했다.
이 탐색 자체가 "초안 → 수정 → 최종"의 앞단 과정에 해당한다.

| 단계 | 모델 / 도구 | 시드 지원 | 채택 여부 및 사유 |
|---|---|---|---|
| 1 | Stable Diffusion 1.5 (로컬) | 지원 | 미채택 — UI 미학습, 추상 이미지·텍스트 전면 깨짐 |
| 2 | SD 1.5 + UI-UX LoRA | 지원 | 미채택 — "전체 폰 목업" 스타일로 편향, 목적과 불일치 |
| 3 | Ideogram | 지원 | 미채택 — 품질은 우수하나 무료 크레딧이 2회 만에 소진 |
| 4 | Bing Image Creator / Leonardo AI | 미지원 | 참고용 — 품질 확인 단계에서만 사용 |
| 5 (최종) | **ChatGPT — GPT Image 2** | 미지원 | **채택** — 한글 렌더링 정확도 최고, 3장을 단일 모델로 통일 가능 |

**시드 미지원에 대한 대응**: GPT Image 계열은 자기회귀(autoregressive) 구조로 SD 계열의
시드 기반 재현 개념과 다르게 설계되어 있고, 공식 API에서 seed 파라미터를 노출하지 않는다.
따라서 시드 고정 대신 아래 두 가지로 재현성·일관성을 확보했다.

1. **스타일 고정 문장** (아래 3절) — 3장에 동일 삽입해 톤 통일
2. **원본 PNG 보존** — 재생성이 아닌 원본 재사용으로 일관성 유지

> 이 대응은 과제 목표 §3-③ *"일관성을 유지하기 위한 방법(시드 고정, 이미지 레퍼런스 등)을
> 설명할 수 있다"* 에 대한 답이다. 시드를 쓸 수 없는 모델에서 **왜, 어떻게 대체했는지**를 함께 남긴다.

---

## 3. 일관성 확보 — 스타일 고정 문장 (Style Lock Sentence)

3개 화면 프롬프트에 아래 문장을 **동일하게** 삽입해 색상·레이아웃 원칙·완성도를 통일했다.
화면별로 달라지는 부분은 "Layout from top to bottom" 아래의 레이아웃 설명뿐이다.

```
Style: clean minimal, modern, soft rounded cards, flat UI design,
mint green accent color (#2EC4B6), crisp white background, subtle drop shadows.

... full-screen mobile interface filling the entire frame edge-to-edge,
like a real screenshot — not a mockup with a phone frame or background margins.

Bottom: fixed navigation bar with 4 icons labeled in Korean
"홈", "크루 목록", "커뮤니티", "마이".

Perfectly aligned typography and spacing, cohesive layout,
looks like a real production mobile app screen.
```

**예외 기록**: 상세 화면(03_detail)은 하단 탭바 대신 화면 폭 전체의 `참여 신청` CTA 버튼으로
대체했다. 상세 화면은 "행동 유도(참여)"가 목적이므로 탭바보다 CTA가 UX상 적절하다고 판단했다.
(→ 나머지 두 화면은 탭바 4개 라벨 고정, 활성 아이콘만 홈/크루 목록으로 다름)

---

## 4. 화면별 프롬프트 로그 (v1 초안 → v2 최종)

각 화면은 **v1(초안)** 을 생성해 문제를 관찰한 뒤, 사유를 반영해 **v2(최종)** 로 개선했다.

### 4-1. 01_main — 홈 화면 (검색바 + 오늘의 추천 크루 카드 + 하단 탭바)

**v1 프롬프트 (초안)**

```
Create a high-fidelity mobile app UI screen design for the home screen of a running crew
matching app called "Project A". Vertical smartphone screen.

Style: clean minimal, modern, soft rounded cards, flat UI design, mint green accent color
(#2EC4B6), crisp white background, subtle drop shadows.

Layout from top to bottom:
1. A sleek search bar with a magnifying glass icon and Korean placeholder text
   "러닝 크루 검색 (예: 강남, 6분 페이스)".
2. App header title in Korean: "Project A: 러닝 크루 매칭".
3. A prominent rounded card titled "오늘의 추천 크루". Inside the card:
   - A photo of diverse young adults running together happily along a river path at sunset.
   - Crew title in bold Korean text: "갑천 러너스".
   - Small cluster of Korean profile avatars representing crew members.
4. Bottom: a fixed navigation bar with 4 icons labeled in Korean "홈", "크루 목록", "커뮤니티",
   "마이". The "홈" icon highlighted in mint accent color, showing it is the active screen.

Vertical 9:16 aspect ratio.
```

**v2 프롬프트 (최종 채택)**

```
Create a high-fidelity mobile app UI screen design for the home screen of a running crew
matching app called "Project A". Make it a vertical smartphone screen, full-screen mobile
interface filling the entire frame edge-to-edge, like a real screenshot — not a mockup with
a phone frame or background margins. Style: clean minimal, modern, soft rounded cards, flat UI
design, mint green accent color (#2EC4B6), crisp white background, subtle drop shadows.
Layout from top to bottom: 1. A sleek search bar with a magnifying glass icon and Korean
placeholder text "러닝 크루 검색 (예: 강남, 6분 페이스)". 2. App header title in Korean:
"Project A: 러닝 크루 매칭". 3. A prominent rounded card titled "오늘의 추천 크루". Inside the
card: - A photo of diverse young adults running together happily along a river path at sunset.
- Crew title in bold Korean text: "갑천 러너스". - Small cluster of Korean profile avatars
representing crew members. - Tag chips: "#6:00 페이스", "#주말러닝", "#초보환영". - Status text:
"최근 활동: 1일 전". - A mint-colored CTA button labeled "더 보기". 4. Bottom: a fixed navigation
bar with 4 icons labeled in Korean "홈", "크루 목록", "커뮤니티", "마이". The "홈" icon highlighted
in mint accent color, showing it is the active screen. Perfectly aligned typography and spacing,
cohesive layout, looks like a real production mobile app screen. Vertical 9:16 aspect ratio.
```

**v1 → v2 변경 사항**

| 구분 | 추가/변경된 요소 | 반영 위치 | 사유 |
|---|---|---|---|
| 레이아웃 지시 | `full-screen edge-to-edge, not a mockup with a phone frame` | 상단 설명부 | v1에 OS 상태바(시간·신호·배터리)가 생성되어 목업처럼 보임 → 실제 스크린샷처럼 프레임 없이 |
| 카드 내부 | 태그 칩 `#6:00 페이스` `#주말러닝` `#초보환영` | 추천 크루 카드 | v1은 사진·제목·아바타만 있어 크루 특징이 안 드러남 |
| 카드 내부 | 상태 텍스트 `최근 활동: 1일 전` | 카드 하단 | 크루 활동 신뢰도 정보 부재 |
| 카드 내부 | CTA 버튼 `더 보기` (mint) | 카드 최하단 | 상세 화면으로 이어지는 유도 요소 부재 |

**결과 확인**: v2 이미지에서 OS 상태바 제거, 태그 3종·최근활동·`더 보기` CTA가 모두 반영됨.

---

### 4-2. 02_list — 크루 목록 화면 (지역·요일 필터 + 크루 목록)

**v1 프롬프트 (초안)**

```
Create a high-fidelity mobile app UI screen design for the crew list screen of a running crew
matching app called "Project A". Vertical smartphone screen.

Style: clean minimal, modern, soft rounded cards, flat UI design, mint green accent color
(#2EC4B6), crisp white background, subtle drop shadows.

Layout from top to bottom:
1. Screen title in bold Korean text "크루 목록", aligned to the left margin.
2. Filter section: a dropdown chip with Korean text "지역: 대전 전체".
3. A sleek search bar with a magnifying glass icon and Korean placeholder text
   "크루 검색 (예: 유성)".
4. Vertically stacked rounded cards with soft edges, each showing a photo of runners on
   different Daejeon trails and parks.
   - Card 1: photo of a running group, bold Korean title "갑천 러너스", small cluster of avatars.
   - Card 2: different photo, bold Korean title "둔산동 씨티 러너스", cluster of avatars.
5. Bottom: fixed navigation bar with 4 icons labeled in Korean "홈", "크루 목록", "커뮤니티",
   "마이". "크루 목록" icon highlighted in mint accent color.

Vertical 9:16 aspect ratio.
```

**v2 프롬프트 (최종 채택)**

```
Create a high-fidelity mobile app UI screen design for the crew list screen of a running crew
matching app called "Project A". Make it a vertical smartphone screen, full-screen mobile
interface filling the entire frame edge-to-edge, like a real screenshot — not a mockup with a
phone frame or background margins.

Style: clean minimal, modern, soft rounded cards, flat UI design, mint green accent color
(#2EC4B6), crisp white background, subtle drop shadows.

Layout from top to bottom:
1. Screen title in bold Korean text "크루 목록", aligned to the left margin.
2. Filter section: a dropdown chip with Korean text "지역: 대전 전체". Below it, a horizontal row
   of 7 day-of-week toggles as individual Korean characters: 월 화 수 목 금 토 일. "월" highlighted
   in solid mint green with white text, the rest in outline or gray default color.
3. A sleek search bar with a magnifying glass icon and Korean placeholder text "크루 검색 (예: 유성)".
4. Vertically stacked rounded cards with soft edges:
   - Card 1: photo of a running group, bold Korean title "갑천 러너스", small cluster of avatars,
     detail text "멤버: 21명 / 다음 러닝: 월요일 7:00PM / 평점: 4.9".
   - Card 2: different photo, bold Korean title "둔산동 씨티 러너스", cluster of avatars,
     detail text "멤버: 14명 / 다음 러닝: 월요일 7:00PM / 평점: 4.7".
   - Card 3 (partially visible at bottom): bold Korean title "유성 트랙 클럽", cluster of avatars.
5. Bottom: fixed navigation bar with 4 icons labeled in Korean "홈", "크루 목록", "커뮤니티",
   "마이". "크루 목록" icon highlighted in mint accent color.

Perfectly aligned typography and spacing, cohesive layout, looks like a real production mobile
app screen. Vertical 9:16 aspect ratio.
```

**v1 → v2 변경 사항**

| 구분 | 추가/변경된 요소 | 반영 위치 | 사유 |
|---|---|---|---|
| 레이아웃 지시 | `full-screen edge-to-edge, not a mockup` | 상단 설명부 | v1에 OS 상태바 생성 → 목업처럼 보임 |
| 필터 구성 | 요일 토글 7개(월~일, "월" 강조) | 필터 섹션 | v1은 지역 필터만 있어 시간대별 검색 불가 |
| 카드 정보 | `멤버 21명 / 다음 러닝 / 평점` 구체 명시 | 카드 하단 | **v1에서 정보 미지정 시 모델이 "총 28명" 같은 임의 정보를 자체 생성** → 원하는 항목을 명시 지정해 통제 |
| 카드 개수 | 3번째 카드(유성 트랙 클럽, 부분 노출) | 리스트 최하단 | v1은 카드 2개로 끝나 스크롤 목록 느낌이 약함 |

**결과 확인**: v1 이미지의 `총 28명`이 v2에서 `멤버: 21명`으로 교정됨. 요일 토글·3번째 카드 반영 확인.

---

### 4-3. 03_detail — 크루 상세 화면 (크루 소개 + 러닝 일정 + 참여 신청 버튼)

**v1 프롬프트 (초안)**

```
Create a high-fidelity mobile app UI screen design for the crew detail screen of a running crew
matching app called "Project A". Vertical smartphone screen.

Style: clean minimal, modern, soft rounded cards, flat UI design, mint green accent color
(#2EC4B6), crisp white background, subtle drop shadows.

Layout from top to bottom:
1. Top: a large header photo showing a running group along a riverside path, with a back arrow
   icon in the top-left corner overlaid on the photo.
2. Crew title in bold Korean text: "갑천 러너스".
3. Crew introduction section with a section title "크루 소개" in bold Korean text, followed by a
   short paragraph of Korean body text describing the crew's running style and atmosphere.
4. Bottom: a fixed mint-green CTA button spanning the width of the screen, labeled "참여 신청" in
   bold white Korean text.

Vertical 9:16 aspect ratio.
```

**v2 프롬프트 (최종 채택)**

```
Create a high-fidelity mobile app UI screen design for the crew detail screen of a running crew
matching app called "Project A". Make it a vertical smartphone screen, full-screen mobile
interface filling the entire frame edge-to-edge, like a real screenshot — not a mockup with a
phone frame or background margins. Style: clean minimal, modern, soft rounded cards, flat UI
design, mint green accent color (#2EC4B6), crisp white background, subtle drop shadows.
Layout from top to bottom: 1. Top: a large header photo showing a running group along a riverside
path, with a back arrow icon in the top-left corner overlaid on the photo. 2. Crew title in bold
Korean text: "갑천 러너스", with a small location tag "대전 · 갑천" below it. 3. Crew stats row:
"멤버: 21명", "평점: 4.9", "개설일: 2023.03" displayed as small icons with text side by side.
4. Crew introduction section with a section title "크루 소개" in bold Korean text, followed by a
short paragraph of Korean body text (friendly, welcoming to beginners, weekly river-side runs).
5. Running schedule section with a section title "러닝 일정" in bold Korean text, showing a rounded
card list of upcoming sessions: - "월요일 7:00PM - 갑천 정규 러닝 (5km)" - "토요일 8:00AM - 주말
장거리 러닝 (10km)". Each schedule item shown as a small rounded card with a calendar icon.
6. Member avatars section with a section title "멤버" in bold Korean text, showing a horizontal row
of small circular Korean profile avatars with a "+16" overflow indicator. 7. Bottom: a fixed,
prominent mint-green CTA button spanning the width of the screen, labeled "참여 신청" in bold white
Korean text. Perfectly aligned typography and spacing, cohesive layout, looks like a real
production mobile app screen. Vertical 9:16 aspect ratio.
```

**v1 → v2 변경 사항**

| 구분 | 추가/변경된 요소 | 반영 위치 | 사유 |
|---|---|---|---|
| 크루 정보 | 위치 태그 `대전 · 갑천` | 제목 하단 | v1은 활동 지역을 알 수 없음 |
| 크루 정보 | Stats 행 `멤버 21명 / 평점 4.9 / 개설일 2023.03` | 위치 태그 아래 | v1은 규모·신뢰도·운영 기간 부재 |
| 신규 섹션 | 러닝 일정 (월 7:00PM 5km / 토 8:00AM 10km) | 크루 소개 아래 | "언제 만나는지"가 없어 참여 전 핵심 의사결정 정보 누락 |
| 신규 섹션 | 멤버 아바타 (아바타 6개 + `+16`) | 러닝 일정 아래 | 실제 활동 인원이 시각적으로 안 보임 |

**결과 확인**: v2 이미지에서 위치 태그·스탯행·러닝 일정 카드·멤버 아바타가 모두 반영됨.

---

## 5. 공통 관찰 — 정보 미명시 시 모델의 임의 생성

3개 화면 모두에서 공통으로 나타난 패턴:

- 프롬프트에 특정 정보(멤버 수, 활동 상태 등)를 명시하지 않으면, 모델은 그 자리를 비워두지 않고
  **임의의 정보를 스스로 생성**했다. 대표 사례: `02_list` v1에서 요청하지 않은 **"총 28명"** 텍스트를
  모델이 자체 생성 → v2에서 `멤버: 21명`으로 명시 지정해 교정.
- 결론: **텍스트 표시가 필요한 요소는 정확한 문구를 프롬프트에 명시적으로 지정**해야 하며, 그렇지
  않으면 부정확한 정보가 결과물에 그대로 포함된다.

> 이 관찰은 과제 목표 §3-① *"프롬프트 구성 요소가 결과물에 미치는 영향을 설명할 수 있다"* 의 근거다.

---

## 6. 후가공 (§4 이미지 후가공 / §6 품질 기준)

- **텍스트 깨짐 대응**: 초기 탐색 모델(SD 1.5, LoRA)은 한글 텍스트가 전면 깨졌다. 후가공으로
  일일이 수정하는 대신, **한글 렌더링 정확도가 가장 높은 GPT Image 2로 모델을 교체**해 깨짐을
  구조적으로 해소했다. 최종 3장은 한글이 정상 렌더링된 상태로 확보되었다.
- **최종 산출물 텍스트 상태**: 최종 v2 3장 모두 외계어·뭉개진 글자 없음(육안 확인).

> 이 항목은 과제 목표 §3-② *"AI 생성 이미지의 문제점(텍스트 깨짐)을 식별하고 수정하는 방법을
> 설명할 수 있다"* 에 대응한다.

---

## 7. 과제 목표(§3) 대응 요약

| 과제 목표 | 대응 근거 (본 문서) |
|---|---|
| ① 프롬프트 구성 요소가 결과물에 미치는 영향 | 4절 화면별 v1→v2 변경표, 5절 임의 생성 관찰 |
| ② 텍스트 깨짐 식별·수정 방법 | 2절 모델 탐색, 6절 후가공 |
| ③ 일관성 유지 방법(시드 고정·레퍼런스) | 2절 시드 미지원 대응, 3절 스타일 고정 문장 |
| ④ 기획→생성→후가공→프로토타입 워크플로우 | 1절 워크플로우 개요, Figma 프로토타입(결과물 ③) |

---

## 8. 산출물 인덱스

| 산출물 | 파일 |
|---|---|
| 홈 화면 | `images/01_main_v1.png` → `images/01_main_v2.png` |
| 목록 화면 | `images/02_list_v1.png` → `images/02_list_v2.png` |
| 상세 화면 | `images/03_detail_v1.png` → `images/03_detail_v2.png` |
| 프로토타입 | Figma 링크 (별도 관리 — "링크 있는 모든 사용자 보기 가능" 권한으로 공개) |
