# 5번째 프로젝트(Echoes of Bond) 추가 구현 계획

`C:\Users\kot77\Desktop\Unity\Bond` 경로의 프로젝트 분석 결과와 지침 문서를 바탕으로, 일관성 있는 기술 백서(`README.md`)를 작성하고 포트폴리오 웹에 연동하는 계획입니다.

## 분석 내용 요약

### 1. 컨셉 기획 (GDD 분석)
- **타이틀**: 에코즈 오브 본드 (Echoes of Bond)
- **장르**: 1인 조종 + 3인 자동 리액션 기반 턴제 전술 로그라이크
- **핵심 차별점**:
  - **이중 리액션 시스템**: 의무적 역할(탱커/딜러/서포터)과 인간적 성향(겁쟁이, 이기주의 등)의 본능적 변수 결합.
  - **전방위 리액션 개입**: 캐릭터 간 연쇄 공격 및 상태 이상 개입 (Chain).
  - **결핍의 압박감**: 스트레스 관리 수치(`Insanity`), 영구 사망 및 자원 결핍.

### 2. 일지 시스템 (Journal System)
- **구조**: `JournalSystem`이 하루 끝 또는 이벤트(전투 종료) 직후 각 `IJournalContentProvider`들로부터 데이터를 수집해 일지를 자동 생성하고, 팝업 렌더링.
- **동작**: 선택지를 임시 보관했다가 일지 종료 시 `IJournalActionHandler`들을 통해 지연 실행(`ExecuteAllDeferredOptions`).

### 3. 전투 및 파이프라인 시스템 (Battle & BattlePipeLine System)
- **전투 제어**: `BattleManager`와 `BattleFlowManager`를 중심으로 턴 및 타격 판정 제어.
- **연산 파이프라인 (`BattlePipeLineSo`)**:
  - `BattleContext` 런타임 데이터를 순차적 단계(`IPipeLineStep<BattleContext>`)를 거치며 점진적으로 가공.
  - `EntryStep` (초기 데미지 산출) ➡️ `EvasionStep` (회피 여부 및 공격 실패 시 시전자 스트레스 +5 가산) ➡️ `CriticalStep` (치명타 시 1.5배 증가 및 스트레스 -5 감산) ➡️ `DefenseStep` (방어력/피해감소 적용 및 소수점 버림 처리) ➡️ `ReactionCall` (리액션 결정 및 초상화 컷신 연출 대기 후 실행) ➡️ `ApplyStep` (스킬 효과 적용).
- **스트레스 연계**: 아군 사망 시 생존 아군 전체 스트레스(+20) 가산 처리 (`HandleCharacterDeath`).

### 4. 외부 시스템 데이터 연동 및 흐름 (Payload)
- **구조**: `ExpeditionPayload`를 통해 마을 ↔ 탐사 ↔ 전투 간 파티 정보, 소모품, 획득 전리품(개척 데이터, 목재, 광석 등) 데이터를 심리스하게 패스스루(Passthrough).
- **흐름**: `BattleFlowManager`가 보상 계산 → `ExpeditionPayload` 가상 계좌에 적립 → `BattleEventProvider`가 보상 수치 파싱하여 일지 리포트 빌드 → `JournalUIView`에서 획득 전리품을 결과창 카드로 시각화.

---

## 백서 마크다운 작성 규격 (01~05 프롬프트 체인 준수)

### 1. 개요 (01 & 02)
- 타이틀, 장르, 제작 인원, 개발 기간, 핵심 스택 메타 표지 카드 구성.
- 프로젝트 정의 및 R&D 배경 (팩트 수치 기반 기재).
- 전체 시스템 데이터 흐름 아키텍처 다이어그램 (Mermaid).

### 2. 시스템 아키텍처 (03)
- 일지 시스템 및 전투 파이프라인의 Sequence / Class 다이어그램 시각화 (Mermaid).

### 3. 핵심 기능 구현 (04)
- 주요 코드 스냅샷 명세 및 상세 위치 기재:
  - `ExpeditionPayload.cs` (데이터 적립 및 패스스루)
  - `BattleEventProvider.cs` (전투 결과 일지 변환 및 발행)
  - `JournalSystem.cs` (일지 지연 선택지 일괄 처리)
  - `BattlePipeLineSo.cs` (점진적 전투 데미지 연산 및 리액션 컷신 연계 파이프라인)
  - `BattleManager.cs` (전투 핵심 비즈니스 로직 및 스트레스 연계)
  - `JournalUIView.cs` (UI Toolkit 결과창 바인딩)

### 4. 고민과 선택 (05)
- 대안 기술 및 아키텍처 구조 비교 표 구성 (예: 데이터 전송 모델, 일지 동기화 방식 등).

### 5. 결과 및 배운 점 (08)
- 정량적 최적화 및 연동 성과, 기술 부채 및 개선 계획 정리.

---

## 제안 변경 사항

### 1. 신규 프로젝트 백서 추가
- **[NEW] [README.md](file:///c:/Users/kot77/Desktop/GitPortfolio/portfolio/project5/README.md)**

### 2. 메인 페이지 반영
- **[MODIFY] [index.html](file:///c:/Users/kot77/Desktop/GitPortfolio/index.html)**
  - 5번째 프로젝트 카드 마크업 추가 (`card-proj-5`).
  - 자바스크립트 `projects` 배열 수정 (`[1, 2, 3, 4]` -> `[1, 2, 3, 4, 5]`).

### 3. 문서 뷰어 페이지 반영
- **[MODIFY] [viewer.html](file:///c:/Users/kot77/Desktop/GitPortfolio/viewer.html)**
  - 좌측 네비게이션 링크 `#5` 추가 (`nav-link-5`).
  - 자바스크립트 `projects` 배열 수정 (`[1, 2, 3, 4]` -> `[1, 2, 3, 4, 5]`).
  - 라우팅 검사 로직 최댓값 수정 (`projectNum > 4` -> `projectNum > 5`).

## 검증 계획
- 로컬 브라우저에서 `index.html`을 구동하여 5번째 프로젝트 카드가 README.md의 타이틀과 스택 태그(`C# · Unity · VContainer · UI Toolkit`)를 정확히 읽어오는지 검증.
- 5번째 프로젝트 상세 뷰어 화면에서 Mermaid 다이어그램 및 코드 블록 가독성 테스트.
