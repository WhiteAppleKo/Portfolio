# extracted_source_facts.md (김우태 역할 및 기여 한정본)

## 1. 프로젝트 역할 및 기여 (Fact)
* **타이틀**: 에코즈 오브 본드 (Echoes of Bond)
* **장르**: 1인 조종 + 3인 자동 리액션 기반 턴제 전술 로그라이크
* **개발 기간**: 2026.04.03 ~ 2026.06.28
* **팀장으로서의 역할**:
  - **프로젝트 기획**: 턴제 전술 및 이중 리액션 결합 컨셉, 그리고 러프한 시스템 기획.
  - **프로젝트 관리**: 주기적인 회의록 작성, 마일스톤 관리 및 파트별 진행 상황 트래킹.
  - **시스템 통합 (System Integration)**: 
    - 팀원들의 개발 코드를 모니터링 및 코드 리뷰하여 전체 아키텍처 정합성을 유지하고 결합 지점을 기술적으로 가이드.
    - 한 모듈의 아웃풋이 다른 모듈의 인풋으로 자연스럽게 연결되도록 인터페이스 규격을 표준화하고 데이터 흐름 조율.
    - 독립된 서브시스템(지도 이벤트, 튜토리얼, 연출 등) 간의 데이터 파이프라인을 통제하여 하나의 완성된 게임 루프로 통합 및 구현 주도.
* **개발자로서의 역할**:
  - **일지 시스템 개발**: 모험 및 전투 기록을 기반으로 일지를 생성 및 제어하고, 사용자가 선택한 결과를 일지 종료 시 병렬 비동기로 일괄 지연 실행하는 공통 제어 시스템 구현.
  - **전투 시스템 및 BattlePipeLine 연동 개발**: `BattleManager`와 `BattleFlowManager`를 설계·연동하고 공용 파이프라인을 응용한 `BattlePipeLine` 전투 연산을 연동하여 전투 제어 및 사망 시 스트레스 가산 기믹 구현.
  - **캠핑 시스템 개발**: 탐사 도중 진입하는 캠프에서 대원들의 상태(부상, 정신력 결핍 등)를 실시간 추적하여 상태에 따라 맞춤형 정비 일지를 동적으로 발행하는 캠핑 시스템 구현.
  - **공용 파이프라인(PipeLine) 시스템 개발**: 순차적 연산의 강결합 한계를 탈피하여 임의의 데이터 흐름에 대해 연산의 명확한 진입과 최종 완결을 시스템 수준에서 강제하고 보장하는 공용 파이프라인 프레임워크 설계 및 구현.

---

## 2. Git Commit Log 기반 본인 개발 궤적 (Fact)
* **Feature/KWT (김우태)**:
  - **일지 시스템 및 UI 연동**: `JournalSystem.cs`, `JournalModel.cs`, `JournalUIView.cs`를 설계·구현하여 모험 기록을 기반으로 한 일지 생성, 비동기 지연 선택지 병렬 처리 및 UI Toolkit 기반 캐릭터 상태 동적 바인딩 구축.
  - **전투 시스템 및 BattlePipeLine 연동**: `BattleManager.cs`, `BattleFlowManager.cs` 및 `BattlePipeLineSo.cs`를 활용하여 턴제 전투 제어, 아군 사망 스트레스 처리 및 전투용 다단계 계산 파이프라인 연동 구현.
  - **공용 파이프라인(PipeLine) 시스템 개발**: `IPipeLineStep.cs` 및 `PipeLineSO.cs`를 설계하여 모든 연산 흐름에 재사용할 수 있는 진입과 완결이 보장된 범용 파이프라인 프레임워크 구축.
  - **씬 간 데이터 연동 (Payload)**: `ExpeditionPayload.cs` 및 `BattleEventProvider.cs`를 설계하여 마을-탐사-전투 씬 간 파티/보급 데이터를 무중단 전달하는 컨테이너 구축 및 전투 보상 일지 자동 발행 흐름 연계.
  - **캠핑 정비 루프 개발**: `CampingSystem.cs` 및 `CampingStageEntry.cs`를 개발하여 대원 상태에 따른 맞춤 정비 일지 동적 발행, 치료 및 씬 안전 퇴장 액션 처리.

---

## 3. 주제별 상세 설계 및 코드 물리 위치 (Fact)

### 3.1. 컨셉 및 시스템 기획 (팀장 기여)
* **기획 문서 정량적 명세**:
  - **기획 문서 총 13개 파일** 설계 및 명세 수립.
  - **00. Concept**: 세계관 및 게임 디자인 방향성 기획 1종.
  - **01. System**: 개요, 기획서, 캐릭터 요소, 인공지능 AI, UIUX 전투 시스템, 캠핑 시스템, 전투 시스템, 이벤트 시스템, 사운드 VFX, 튜토리얼 작동 기획 등 시스템 설계서 10종.
  - **WorkGuide**: 개발 협업 룰 및 태스크 카드 포맷 가이드 2종.
* **기획 및 시스템 통합 설계 핵심**:
  - **이중 리액션 기획**: 역할 의무(탱커/딜러/서포터)와 개인 성향(겁쟁이 등)의 충돌 메커니즘 구상.
  - **리더의 일지(Journal) 연계 기획**: 모험과 전투가 끝난 뒤 그에 대한 결과가 보고서 형식의 서사 일지로 환류되는 내러티브 게임 루프 기획.
  - **모듈 간 인터페이스 규격 설계**: 개별 개발 컴포넌트 간 아웃풋-인풋 데이터 매핑 규격을 사전에 정의하여, 서브시스템 간 흐름이 정체 없이 유기적으로 연쇄되도록 조율.
* *[이미지 필요: 리액션 에디터 UI 기획 목업]*
* *[이미지 필요: 전투 화면 리액션 뱃지 및 스트레스 경고 UI]*
* *[이미지 필요: 캠핑 화면 리더의 일지 및 보급 배분 UI]*

### 3.2. 일지 시스템 - 개발자 기여
* **핵심 스크립트**: `Assets/96. WT/JournalSystem/Scripts/Logic/JournalSystem.cs`
* **사용자 경험**:
  - 전투/탐사가 끝나면 딱딱한 수치창 대신 리더의 서사적 보고서(일지) 형태로 팝업되어 탐사 결과의 스토리를 순차 체감하고 하단 선택지를 통해 의사결정을 조작함.
* **시스템 내부 동작**:
  - 각 서브시스템(`IJournalContentProvider`)의 리포트 데이터를 동적 버퍼링.
  - 유저 선택 시 즉시 변경을 방지하고 버퍼에 기록을 유예한 뒤, 일지 최종 종료(`IsLastPage`) 타이밍에 `IJournalActionHandler`들을 통해 일괄 비동기 병렬 실행(`UniTask.WhenAll`)하여 데이터 안정성 보장.
* **코드 위치**:
  - `Assets/96. WT/JournalSystem/Scripts/Logic/JournalSystem.cs` (지연 실행 및 루프 제어)

### 3.3. 전투 시스템 및 BattlePipeLine 연동 - 개발자 기여
* **전투 핵심 스크립트**: `Assets/02. Scripts/BattleSystem/BattleManager.cs`, `BattleFlowManager.cs`
* **사용자 경험**:
  - 타격 실패 시 시전자의 스트레스 게이지가 누적되고 치명타 시 해소되는 캐릭터 심리 변화 실시간 인지.
  - 리액션 발동 시 연출 컷신이 팝업된 후 개입 공격이 실행되는 연쇄 전투 연출 및 아군 사망 시 연쇄 스트레스(+20) 전이 효과 체감.
* **시스템 내부 동작**:
  - `BattleManager`와 `BattleFlowManager`가 전투의 시작/종료 턴 루프 실행.
  - `BattlePipeLineSo` 파이프라인 프레임워크를 기반으로 런타임 데이터인 `BattleContext`를 순차 전달하여 스탯 보정, 회피 확률 계산, 크리티컬 계산, 방어력 피해 감쇄(FloorToInt 버림 처리)를 점진적으로 연산하고 리액션 초상화 연출(`ShowAsync`) 대기 후 연동 실행.
* **코드 위치**:
  - `Assets/03. PipeLine/BattlePipeLineSo.cs` (전투 파이프라인 구현체)
  - `Assets/02. Scripts/BattleSystem/BattleManager.cs` (사망 시 스트레스 연계 로직)

### 3.4. 공용 파이프라인 시스템 - 개발자 기여
* **핵심 스크립트**: `Assets/03. PipeLine/PipeLineBase/` (`IPipeLineStep.cs`, `PipeLineSO.cs`)
* **사용자 경험**:
  - 다단계 복합 수치(전투 데미지 등)가 중간에 꼬이거나 끊기는 현상 없이 정교하고 신속하게 계산되어 즉각적인 연산 결과 피드백 확인.
* **시스템 내부 동작**:
  - 임의의 데이터 타입에 대해 개별 Step들을 순서대로 통과시켜, 계산 연산의 명확한 진입(시작)과 끝(최종 완결)을 프레임워크 수준에서 강제하고 보장하는 공용 아키텍처 수립.
  - 계산 도중 특정 조건 만족 시 파이프라인 브레이크(`ShouldBreak`) 분기를 통해 불필요한 계산 차단 및 자원 낭비 방지.
* **코드 위치**:
  - `Assets/03. PipeLine/PipeLineBase/PipeLineSO.cs` (공용 파이프라인 베이스)

### 3.5. 캠핑 시스템 - 개발자 기여
* **핵심 스크립트**: `Assets/96. WT/CampingSystem/Scripts/CampingSystem.cs`
* **사용자 경험**:
  - 모닥불 캠프 화면에서 대원들의 결핍 수치(부상, 정신 상태)를 보며 남은 붕대/진정제를 선별 분배하거나, 자원을 아끼고 패널티를 감수하는 서바이벌 자원 배분 조작.
* **시스템 내부 동작**:
  - `GenerateCampingReport()`가 대원의 HP 및 Insanity 결핍 상태를 전수 검사하여 조건 매칭 유닛에 맞게 붕대 치료 일지 및 정신력 진정 일지를 동적으로 조립하고 발행.
  - 선택지 조작 결과를 버퍼링한 뒤 씬 귀환(`ACTION_RETURN_MAP`) 액션 처리 시 데이터 반영 및 씬 언로드 진행.
* **코드 위치**:
  - `Assets/96. WT/CampingSystem/Scripts/CampingSystem.cs` (정비 리포트 생성 및 전리품 체크)

### 3.6. 외부 데이터 연동 및 UI 시각화 - 개발자 기여
* **데이터 컨테이너**: `Assets/02. Scripts/Town/Embark/ExpeditionPayload.cs`
* **사용자 경험**:
  - 전투 승리 시 전리품 획득 상황과 대원의 생존 체력/스트레스 바 게이지가 연출 카드와 함께 실시간 반영되는 화면 피드백 체감.
* **시스템 내부 동작**:
  - VContainer DI 컨테이너를 통해 `ExpeditionPayload`가 씬 전환 간 소멸하지 않고 파티 데이터와 보급품, 누적 보상 정보를 무중단 전송(Passthrough).
  - 전투 종료 시 `BattleEventProvider`가 보상 및 사망 상태를 가공하여 동적 일지 리포트로 발행하며, `JournalUIView`가 UI Toolkit을 활용해 동적 슬롯 카드를 빌드하고 스타일시트의 `Length.Percent`를 이용해 체력/스트레스 바의 길이를 퍼센트로 동적 제어함.
* *[이미지 필요: 전투 결과창 보상 및 캐릭터 카드 UI Toolkit 화면]*
* **코드 위치**:
  - `Assets/02. Scripts/Town/Embark/ExpeditionPayload.cs` (데이터 컨테이너)
  - `Assets/96. WT/JournalSystem/Scripts/Logic/BattleEventProvider.cs` (전투 결과 일지 발행자)
  - `Assets/96. WT/JournalSystem/Scripts/UI/JournalUIView.cs` (UI Toolkit 동적 결과창 바인딩)
