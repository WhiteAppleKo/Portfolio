# XR Lab 기술 백서 (README.md) 개선 작업 계획서

본 문서는 `XR Lab` 프로젝트의 기술 명세서(README.md)를 Project 1, 2와 일관된 스타일로 리빌드하기 위한 구체적인 작업 계획서입니다.

---

## 1. 목적 및 통일성 기준
* **통일성 대상**: [project1/README.md](file:///C:/Users/kot77/Desktop/GitPortfolio/portfolio/project1/README.md), [project2/README.md](file:///C:/Users/kot77/Desktop/GitPortfolio/portfolio/project2/README.md)
* **디자인 및 레이아웃**: 3색 2폰트 규칙 적용, `image-card-text hover-image` 레이아웃 적극 활용, Mermaid 네이티브 렌더링 유지.
* **구조적 기준**: 5단계 백서 가이드라인 준수 (개요 -> 아키텍처 -> 구현 -> 고민과 선택 -> 회고)

---

## 2. 세부 개선 계획

### ① 개요 (What & Why) 개편
* **팀원 구성 및 역할**: 2인 팀에 알맞은 Mermaid 다이어그램 추가.
  * 김우태 (본인 - 메인 기획/개발), 팀원 (서브 개발/리소스 관리)
* **R&D 동기 분리**: 타이트한 18일 일정 내 물리 검증 및 셰이더 시각화 R&D 동기를 명확히 제시.

### ② 시스템 구조 (Architecture) 가독성 향상
* **다이어그램 흐름**: flowchart TD에서 `flowchart LR`로 변경하여 세로 스크롤 최소화.
* **레이아웃 매칭**: 이미지 스냅샷에 `hover-image` 래퍼 클래스 적용.

### ③ 핵심 기능 구현 (Implementation) 안정성 확보
* **꺾쇠 파싱 결함 방어**: C# 제네릭 표기(`<List>`, `<GameObject>`) 주변에 HTML 파싱 오류 방지 지침 추가 및 이스케이프 적용.
* **코드 블록 가독성**: 핵심 구문 형광 강조 가이드 추가 및 한글 주석 번호화 정비.

### ④ 고민과 선택 (Trade-offs)
* **대안 대조**: 일반 소켓(대안 A) vs 커스텀 Lock Grid 소켓(대안 B) 대안 비교 및 결정 Rationale을 명확한 레이아웃으로 대조.

### ⑤ 프로젝트 회고 (Retrospective)
* **기호 일관성**: 성과(`✓`), 한계점(`△`), 개선 계획(`→`) 3단 마커 분류법 통일 적용.

---

## 3. 마일스톤 및 일정
| 단계 | 작업 내용 | 예상 소요 |
| :--- | :--- | :--- |
| **Step 1** | 계획서 피드백 수렴 및 데모 영상 링크 주소 확인 | 즉시 |
| **Step 2** | `project3/README.md` 전면 개정 및 리빌드 수행 | 10분 |
| **Step 3** | 교차 검증 및 통일성 체크 | 5분 |

---

## 4. 확인 필요 사항
* **데모 영상 링크**: 기존 임시 YouTube 링크(`https://youtube.com/watch?v=SomeXRVideoUrl`)를 그대로 유지합니까, 아니면 실제 데모 비디오 링크가 존재합니까?
