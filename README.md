# [Basic Term Project_5th Group] 작업 로그 - (Project A) AI 기반 UI/UX 디자인 시안 제작

- 제출물 구분: 결과물 ② 작업 로그 문서 (1개)  
- 선택한 프로젝트: Project A  
- 프로젝트 주제: 러닝 크루 매칭 앱 화면 시안 제작  
- 저장소:   
- 문서 설명: 이 문서는 `기획 → 프롬프트 → 이미지 생성 → 후가공 → 프로토타입`으로 이어지는 AI 협업 워크플로우의 과정 기록이다. 평가 대상은 결과 이미지의 완성도가 아니라 "어떤 단어를 넣었더니 무엇이 어떻게 달라졌는가"이므로, 화면별로 프롬프트 원문(v1 → v2)과 변경 사유·결과 차이를 그대로 남긴다.  

---

## 0. 사용 도구 (과제 §6 — 도구 사용 및 명)

| 구분 | 도구 | 계정 구분 | 사용 이유 | 제출물 활용 |
|---|---|---|---|---|
| 이미지 생성/수정 | **ChatGPT — GPT Image 2<br> (gpt-image-2)** | ChatGPT 유료 플랜 | 한글 텍스트 렌더링 정확도가 검토 대상 중 가장 높음. 3장 전체를 단일 모델로 통일 가능 | 최종 3장 전체 생성 |
| 이미지 생성 (검토·미채택) | Stable Diffusion 1.5 (로컬, AUTOMATIC1111) / UI-UX LoRA / Ideogram / Bing Image Creator / Leonardo AI | — | 모델 비교 검토 단계에서만 사용, 최종 산출물에는 미포함 | N/A |
| 프로토타입 | **Figma** | 무료 플랜 | 과제 권장 도구 | Hotspot, Overlay 후가공 |
| Html/Css 변환 | **Claude Opus 4.8** | Pro 구독형 | 빌드업(코딩) | AI 생성물을 활용한 html/css 기반 사이트 구축 |
> (§6 제약 사항) 외부 레퍼런스 이미지를 사용한 경우 이 표 아래에 출처(작성자·URL)를 기록해야 한다. 타인 작업물(Pinterest·Dribbble) 캡처 제출은 금지 사항이다.
> *→ 본 제출물에서 외부 레퍼런스 이미지나 타인 작업물을 사용하지 않았습니다. 화면 레이아웃은 프롬프트 텍스트만으로 생성했으며, 외부 디자인 이미지를 레퍼런스로 입력하지 않았습니다.*
---

## 0-1. 산출물 구성

| 항목 | 버전 | 내용 | 제출 형태 |
|---|---|---|---|
| 레이아웃 이미지 | v1 | main, list, detail 레이아웃 3장  | .png (이미지) |
| 레이아웃 이미지 | v2 | main, list, detail 레이아웃 3장<br>(main과 list는 이게 최종)  | .png (이미지) |
| 레이아웃 이미지 | 최종 | detail 레이아웃 1장  | .png (이미지) |
| 작업 로그 | 최종 | 프롬프트 변경 이력 · 후가공 기록 · 프로토타입 명세 | README.md (문서) |
| Figma 링크 | 최종 | 그림 3장을 얹고 화면 전환을 연결한 시연용 프로토타입 | 공유 링크 (보기 권한 공개) |
| 보너스 과제 제출 | 최종 | 시연용 프로토타입 html 사이트앱 | RuncrewApp.html (파일) |
> Figma 링크: https://www.figma.com/design/XnTj4Hm1SKSKtktYlxihJu/%EC%A0%9C%EB%AA%A9-%EC%97%86%EC%9D%8C?node-id=0-1&p=f&t=FicCzWjq2c0L8YKw-0

## 1. 워크플로우 개요

```
[기획-김동호&팀원]  서비스 컨셉("러닝크루모집앱" 선정)·화면 3역할 정의 (메인 / 목록 / 상세)
   │
[모델 탐색-육민호]  SD 1.5 → LoRA → Ideogram → Bing/Leonardo → GPT Image 2 (최종)
   │
[프롬프트 최적화-육민호]  화면별 v1(초안) → v2(최종), 사유·결과 차이 기록
   │
[일관성 확보-육민호]  시드 미지원 → '스타일 고정 문장' + 원본 PNG 보존
   │
[결과물 검토-김동호] 레이아웃 최종화 결정(main, list - 최종화, detail - 보완)
   |
[이미지 수정-육민호] 이미지 수정(v2 → 최종), 프롬프트 기록
   │
[프로토타입-김주원]  Figma에 3장 배치 + 투명 Hotspot으로 화면 전환 연결
   | 
[후가공-김주원] 텍스트 깨짐·부자연 요소(날짜, 숫자 로직) 대응
   | 
[보너스-김동호] 생성된 Asset 기반 html/css 빌드업 코딩
```
---
## 2. 레이아웃 생성(담당자: 육민호)

## 2-1. 이미지 생성 모델 선정/프롬프팅 엔지니어링 기법

   (1) 모델 선정

| 단계 | 모델 / 도구 | 시드 지원 | 채택 여부 |
|---|---|---|---|
| 1 | Stable Diffusion 1.5 (로컬, AUTOMATIC1111 WebUI) | 지원 (직접 지정 가능) | 미채택 — UI 미학습 모델로 추상 이미지 출력, 텍스트 전면 깨짐 |
| 2 | SD 1.5 + UI-UX LoRA | 지원 | 미채택 — LoRA가 "전체 폰 목업" 스타일로 편향 학습되어 있어 목적(고립 컴포넌트/실제 화면 재현)과 불일치 |
| 3 | Ideogram | 지원 (무료 계정도 생성 후 확인 가능) | 미채택 — 품질은 우수했으나 무료 크레딧이 2회 생성 만에 소진되어 3장 전체 작업에 부적합 |
| 4 | Bing Image Creator / Leonardo AI (GPT Image 2 경유) | 미지원 | 참고용 — 품질 확인 단계에서 사용 |
| 5 (최종) | ChatGPT — gpt-image-2 | 미지원 | **채택** — 한글 텍스트 렌더링 정확도가 가장 높고, 3장 전체를 하나의 모델로 통일 가능 |

   (2) 톤 통일 방법

시드(seed) 고정이 불가능한 모델(GPT Image 2)을 최종 채택했기 때문에, 시드 대신 3장에 동일하게 삽입한 **스타일 고정 문장**으로 색상·레이아웃 원칙·완성도를 통일했다.

1. **스타일 고정 문장** — 3장 모두 동일한 스타일·레이아웃 원칙 문장을 사용해 톤을 통일.
```
Style: clean minimal, modern, soft rounded cards, flat UI design, mint green accent color (#2EC4B6), crisp white background, subtle drop shadows.

Full-screen mobile interface filling the entire frame edge-to-edge, like a real screenshot — not a mockup with a phone frame or background margins.

Bottom: a fixed navigation bar with 4 icons labeled in Korean "홈", "크루 목록", "커뮤니티", "마이". Keep the navigation bar visible on every screen. Highlight only the active tab using the mint accent color while keeping the other tabs neutral gray.

Primary CTA buttons should appear as floating rounded buttons positioned above the bottom navigation bar, with clear spacing separating the CTA from the navigation bar to avoid visual clutter. The CTA should not span the full screen width and should feel visually independent from the navigation bar.

Profile avatars should be rendered as large, high-resolution circular portraits with sharp facial details, consistent sizing, and realistic lighting.

Perfectly aligned typography and spacing, cohesive layout, balanced visual hierarchy, consistent iconography, looks like a real production mobile app screen.
```
2. **원본 파일 보존** — 생성된 원본 PNG를 그대로 보관하여, 재생성이 아닌 원본 재사용으로 일관성을 유지

## 2-2. 레이아웃의 구성
   (1) 서비스 컨셉
   > 대전 지역 기반 러닝 크루 매칭 앱. 혼자 뛰기 어려운 사용자가 거주 지역·요일·페이스 조건으로 크루를 찾아 참여 신청까지 이어지도록 하는 서비스다.

   (2) 화면 구성
   
| 화면 | 역할 | 핵심 구성 요소 |
|---|---|---|
| `01_main` | 메인 |  - 상단 검색바  <br>   - 중단 추천 그룹(소속 크루 아이콘, 총원) 표시<br>   - 하단 네비게이션바   |
| `02_list` | 목록 | - 지역 필터, 요일 토글, 검색바<br> - 크루 카드 리스트<br> - 하단 네비게이션바 |
| `03_detail` | 상세 | - 크루 소개(대표이미지), 스탯, 러닝 일정, 멤버, 소개글<br> - '참여 신청'<br> - 하단 네비게이션바|

   (3) 레이아웃 연결 구조 (§2, §4)
```
화면 흐름: [메인] → '더 보기' 클릭 → [목록] → 항목('갑천 러너스') 클릭 → [상세] → 뒤로가기 → [목록]
                                         └──→ 홈 버튼(하단 바) 클릭 → [메인]└──→ 홈 버튼(하단 바) 클릭 → [메인]

```

## 2-3. 레이아웃 프롬프트
## (1) Main
# v1 prompt (Original)
```
Create a high-fidelity mobile app UI screen design for the home screen of a running crew matching app called "Project A". Vertical smartphone screen.

Style: clean minimal, modern, soft rounded cards, flat UI design, mint green accent color (#2EC4B6), crisp white background, subtle drop shadows.

Layout from top to bottom:
1. A sleek search bar with a magnifying glass icon and Korean placeholder text "러닝 크루 검색 (예: 강남, 6분 페이스)".
2. App header title in Korean: "Project A: 러닝 크루 매칭".
3. A prominent rounded card titled "오늘의 추천 크루". Inside the card:
   - A photo of diverse young adults running together happily along a river path at sunset.
   - Crew title in bold Korean text: "갑천 러너스".
   - Small cluster of Korean profile avatars representing crew members.
4. Bottom: a fixed navigation bar with 4 icons labeled in Korean "홈", "크루 목록", "커뮤니티", "마이". The "홈" icon highlighted in mint accent color, showing it is the active screen.

Vertical 9:16 aspect ratio.
```
# v1 → v2 변경 사항

| 구분 | 추가/변경된 요소 | 반영 위치 | 추가 사유 |
|---|---|---|---|
| 레이아웃 지시 | `full-screen mobile interface filling the entire frame edge-to-edge, like a real screenshot — not a mockup with a phone frame or background margins.` | 상단 전체 설명부 | v1 결과에 OS 상태바(시간·신호·배터리)가 함께 생성되어 목업처럼 보임 → 실제 스크린샷처럼 프레임 없이 꽉 채우도록 명시 |
| 카드 내부 요소 | Tag chips: `"#6:00 페이스"`, `"#주말러닝"`, `"#초보환영"` | "오늘의 추천 크루" 카드 내부 | v1은 카드에 사진·제목·아바타만 있어 크루 특징이 드러나지 않음 → 정보성 태그 추가 |
| 카드 내부 요소 | Status text: `"최근 활동: 1일 전"` | 카드 하단 | 크루 활동 신뢰도를 보여줄 정보 부재 → 추가 |
| 카드 내부 요소 | CTA 버튼: `"더 보기"` (mint 색상) | 카드 최하단 | 다음 행동(상세 화면 이동)으로 이어지는 유도 요소 부재 → 추가 |

---
## (2) List
# v1 prompt (Original)
```
Create a high-fidelity mobile app UI screen design for the crew list screen of a running crew matching app called "Project A". Make it a vertical smartphone screen, full-screen mobile interface filling the entire frame edge-to-edge, like a real screenshot — not a mockup with a phone frame or background margins.

Style: clean minimal, modern, soft rounded cards, flat UI design, mint green accent color (#2EC4B6), crisp white background, subtle drop shadows.

Layout from top to bottom:
1. Screen title in bold Korean text "크루 목록", aligned to the left margin.
2. Filter section: a dropdown chip with Korean text "지역: 대전 전체". Below it, a horizontal row of 7 day-of-week toggles as individual Korean characters: 월 화 수 목 금 토 일. "월" highlighted in solid mint green with white text, the rest in outline or gray default color.
3. A sleek search bar with a magnifying glass icon and Korean placeholder text "크루 검색 (예: 유성)".
4. Vertically stacked rounded cards with soft edges, each showing a photo of runners on different Daejeon trails and parks:
   - Card 1: photo of a running group, bold Korean title "갑천 러너스", small cluster of Korean profile avatars, detail text "멤버: 21명 / 다음 러닝: 월요일 7:00PM / 평점: 4.9".
   - Card 2: different photo, bold Korean title "둔산동 씨티 러너스", cluster of avatars, detail text "멤버: 14명 / 다음 러닝: 월요일 7:00PM / 평점: 4.7".
   - Card 3 (partially visible at bottom): different photo, bold Korean title "유성 트랙 클럽", cluster of avatars.
5. Bottom: fixed navigation bar with 4 icons labeled in Korean "홈", "크루 목록", "커뮤니티", "마이". "크루 목록" icon highlighted in mint accent color.

Perfectly aligned typography and spacing, cohesive layout, looks like a real production mobile app screen. Vertical 9:16 aspect ratio.
```
# v1 → v2 변경 사항

| 구분 | 추가/변경된 요소 | 반영 위치 | 추가 사유 |
|---|---|---|---|
| 레이아웃 지시 | `full-screen edge-to-edge, not a mockup with a phone frame` | 상단 전체 설명부 | v1에 OS 상태바가 함께 생성되어 목업처럼 보임 → 프레임 없이 꽉 채우도록 명시 |
| 필터 구성 | 요일 토글 7개(월~일, "월" 강조) | 필터 섹션 | v1은 지역 필터만 있어 시간대별 검색이 불가능 → 요일 필터 추가 |
| 카드 정보 | `멤버: 21명 / 다음 러닝: 월요일 7:00PM / 평점: 4.9` 등 구체적 detail text | 카드 하단 | v1에서는 정보를 지정하지 않자 모델이 "총 28명"처럼 임의의 부정확한 정보를 자체 생성 → 원하는 정보 항목을 명시적으로 지정해 통제 |
| 카드 개수 | 3번째 카드(유성 트랙 클럽, 부분 노출) | 리스트 최하단 | v1은 카드 2개로 끝나 스크롤 가능한 목록이라는 느낌이 약함 → 3번째 카드 부분 노출로 리스트 지속성 암시 |

---
## (3) Detail
# v1 prompt (Original)
```
Create a high-fidelity mobile app UI screen design for the crew detail screen of a running crew matching app called "Project A". Vertical smartphone screen.

Style: clean minimal, modern, soft rounded cards, flat UI design, mint green accent color (#2EC4B6), crisp white background, subtle drop shadows.

Layout from top to bottom:
1. Top: a large header photo showing a running group along a riverside path, with a back arrow icon in the top-left corner overlaid on the photo.
2. Crew title in bold Korean text: "갑천 러너스".
3. Crew introduction section with a section title "크루 소개" in bold Korean text, followed by a short paragraph of Korean body text describing the crew's running style and atmosphere.
4. Bottom: a fixed mint-green CTA button spanning the width of the screen, labeled "참여 신청" in bold white Korean text.

Vertical 9:16 aspect ratio.
```
# v1 → v2 변경 사항

| 구분 | 추가/변경된 요소 | 반영 위치 | 추가 사유 |
|---|---|---|---|
| 크루 정보 | 위치 태그 `대전 · 갑천` | 제목 하단 | v1은 크루가 어느 지역에서 활동하는지 알 수 없음 → 위치 정보 추가 |
| 크루 정보 | Stats 행 `멤버: 21명 / 평점: 4.9 / 개설일: 2023.03` | 제목 하단, 위치 태그 아래 | v1에서 크루의 규모·신뢰도·운영 기간을 알 수 없음 → 추가 |
| 신규 섹션 | 러닝 일정 섹션 (`월요일 7:00PM 갑천 정규 러닝(5km)`, `토요일 8:00AM 주말 장거리 러닝(10km)`) | 크루 소개 아래 | 상세 화면에서 "언제 만나는지"가 없어 참여 신청 전 핵심 의사결정 정보 누락 → 일정 섹션 추가 |
| 신규 섹션 | 멤버 아바타 섹션 (아바타 6개 + `+16`) | 러닝 일정 아래 | 크루 소개 텍스트만으로는 실제 활동 인원이 보이지 않음 → 추가 |

# v2 → v3 변경 사항

| 구분 | 추가/변경된 요소 | 반영 위치 | 추가 사유 |
|------|------------------|----------|----------|
| 하단 내비게이션 | 고정 하단 탭바 추가 (홈 / 크루 목록 / 커뮤니티 / 마이) | 화면 하단 | 모든 화면에서 일관된 내비게이션을 제공하고 화면 간 이동성을 향상 |
| CTA 개선 | 참여 신청 버튼을 Floating 형태로 변경하고 탭바와 충분한 간격 확보 | 화면 하단 | CTA와 탭바가 붙어 보이는 시각적 혼잡(Clutter)을 줄이고 버튼의 우선순위를 높임 |
| 멤버 섹션 | 프로필 아바타를 더 큰 크기와 높은 해상도로 개선 | 멤버 섹션 | 얼굴 식별성을 높이고 실제 서비스 수준의 UI 품질 향상 |
| 레이아웃 | CTA와 탭바의 시각적 계층 구조 강화 | 화면 하단 | 주요 액션 버튼과 내비게이션을 명확히 구분하여 사용성을 개선 |
| 스타일 일관성 | Style Lock Sentence를 강화하여 레이아웃 및 UI 요소를 고정 | 전체 화면 | 모든 화면에서 동일한 디자인 시스템과 완성도를 유지하기 위함 |

---

