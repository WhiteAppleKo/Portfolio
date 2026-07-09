# Vampire Survivor Like 기술 백서 (README.md) 개선 작업 계획서

본 문서는 `Vampire Survivor Like` 프로젝트의 기술 명세서(README.md)를 Project 1, 2, 3과 일관된 스타일로 리빌드하기 위한 구체적인 작업 계획서입니다.

---

## 1. 목적 및 통일성 기준
* **통일성 대상**: [project1/README.md](file:///C:/Users/kot77/Desktop/GitPortfolio/portfolio/project1/README.md), [project2/README.md](file:///C:/Users/kot77/Desktop/GitPortfolio/portfolio/project2/README.md), [project3/README.md](file:///C:/Users/kot77/Desktop/GitPortfolio/portfolio/project3/README.md)
* **디자인 및 레이아웃**: 3색 2폰트 규칙 적용, `image-card-text hover-image` 레이아웃 적극 활용, Mermaid 네이티브 렌더링 유지.
* **구조적 기준**: 5단계 백서 가이드라인 준수 (개요 -> 아키텍처 -> 구현 -> 고민과 선택 -> 회고)

---

## 2. 세부 개선 계획

### ① 아키텍처 및 다이어그램 가로 레이아웃 전환
* 1.3절 (전체 아키텍처), 3.1절 (2단계 필터링) ➡️ `flowchart LR` 전환하여 세로 스크롤 최소화.
* 5.2절 (하이브리드 UI FSM) ➡️ 원천 소스의 '3단계 UI 발전사'(`flowchart LR`)로 수정하여 임의 FSM 상태 왜곡 차단.

### ② 미디어 카드 (`image-card-text hover-image`) 전환 및 추가
* 기존 HTML `<img src>` ➡️ `image-card-text hover-image` 포맷 변환하여 마우스 클릭 고정 및 휠 줌 기능 연동.
* 2.2절 하단: `page_15_img_1.png` 적용.
* 3.1절 하단: `page_27_img_1.png` (Cube 좌표), `page_32_img_1.png` (2중 탐색) 이미지 카드 추가.
* 5.2절 하단: `page_36_img_1.png` (UI 포커스), `page_68_img_1.png` (임포터 툴) 이미지 카드 적용.

### ③ 팩트 교차 검증 및 정제
* 6.1절 드로우콜 수치: "도입 전 평균 220회" ➡️ "개별 타일 다수 배치 시 드로우콜 급증"으로 정제하여 팩트 신뢰성 확보.

### ④ 코드 블록 정비
* C# 제네릭 꺾쇠 괄호 파싱 방지(`&lt;`, `&gt;`) 이스케이프 적용.
* 한글 주석 번호화 정비.

---

## 3. 마일스톤 및 일정
| 단계 | 작업 내용 | 예상 소요 |
| :--- | :--- | :--- |
| **Step 1** | 계획서 수립 및 승인 | 완료 |
| **Step 2** | `project4/README.md` 전면 개정 및 리빌드 수행 | 즉시 |
| **Step 3** | 교차 검증 및 통일성 체크 | 5분 |
