# 📂 뱀서라이크 프로젝트 기술 백서 원천 팩트 데이터 (Project 4)

본 문서는 `뱀서라이크.pdf`에서 추출된 원천 팩트를 정리한 기술 문서 작성용 원시 데이터 소스입니다. 가공되거나 왜곡된 수치를 배제하고 원본 텍스트에 기재된 정보만을 구조화했습니다.

---

## 1. 프로젝트 기본 메타데이터
* **프로젝트명**: 뱀서라이크 (Vampire Survivor Like 모작)
* **개발 인원**: 1명 (개인 프로젝트)
* **개발 기간**: 2025.12 ~ 2026.02 (약 3개월)
* **핵심 기술 스택**: Unity 6000.3.8f1, C#, Shapes Asset, HexGrid, Sourcetree

---

## 2. 5단계 백서 작성을 위한 원천 팩트 분류

### [1단계] 개요 (What & Why)
* **What (프로젝트 정의)**: 서바이버 로그라이크 장르의 본질인 '성장의 축적'과 '물량의 쾌감'을 표현하기 위해, Unity의 Shapes 에셋을 통한 고품질 벡터 그래픽 시각화 및 육각 그리드(HexGrid) 좌표계 물리 연산 아키텍처를 구현한 1인 개발 R&D 프로젝트.
* **Why (R&D 동기)**: 조작 피로도를 줄이기 위해 입력을 '이동'으로 최소화하되, 대규모 적 몬스터의 등장 상황 속에서 무기별 사거리와 안전 구역의 시각적 가독성을 극대화하고, 데이터 주도(CSV Importer) 방식으로 무기와 증강 시스템을 코드 수정 없이 무한히 확장할 수 있는 아키텍처 R&D.

### [2단계] 시스템 구조 (Architecture)
* **Data - Logic - Visual (DLV) 분리 아키텍처**:
  * **Data**:
    * `PureData`: 기획자가 작성한 불변의 설계 데이터(CSV, ScriptableObject), Immutable(readonly 적용) 및 Logic-Free 보장. `PureDataBase`가 `Dictionary<ID, PureData>` 형태로 데이터를 보관.
    * `RuntimeData`: PureData를 참조하며 실제 게임 중 변화하는 데이터. Observable(값 변경 시 C# event 발송) 및 Self-Update(Tick 메서드로 시간/상태 갱신) 규칙 준수.
  * **Logic**: RuntimeData를 기반으로 실제 연산 및 상태 판단 수행. Visual 인터페이스를 호출하며, Rigidbody, NavMeshAgent, transform.position 등 물리/이동 컴포넌트를 직접 제어하지 않음.
  * **Visual**:
    * `Object Visual (Interface)`: Logic과 Visual의 의존성 역전(DIP)을 담당하며 구체적인 구현(2D/3D, NavMesh/Translate)을 노출하지 않음.
    * `UI Visual`: Observer 패턴을 활용해 Logic을 거치지 않고 Model(RuntimeData)의 이벤트를 직접 구독하여 Canvas UI를 갱신.

### [3단계] 핵심 기능 및 구현 (Implementation)
* **HexGrid & Cube 좌표계**:
  * **좌표 제어**: 육각형 그리드를 판정과 배경의 최소 단위로 설정. 3축 Cube 좌표계 제약 조건 `q + r + s = 0`을 강제하여 삼각함수와 제곱근(sqrt) 연산 없이 정수 연산만으로 거리 계산, 인접 이동, 대칭 연산을 고속 처리.
  * **맨해튼 거리 공식**: 두 타일 간 거리를 $Distance = (|dq| + |dr| + |ds|) / 2$ 공식을 통해 부동소수점 오차 없이 단순 정수 연산으로 구하여 런타임 성능 극대화.
  * **OverlapHexGrid (2중 탐색)**: 1단계 `Physics.OverlapSphere`를 사용해 물리 엔진 레벨에서 넓은 범위의 후보군을 먼저 추출한 후 ➡️ 2단계 `GetHexDistance` 정밀 육각 거리 비교를 통해 범위 내 최종 대상 타겟 리스트를 반환하는 고속 필터링 구현.
* **AutoAttack & Weapon 추상화**:
  * **무기 추상화**: 공통 쿨타임, 스탯 재연산 로직을 포함한 `Weapon` 부모 추상 클래스를 설계하고, 자식 무기(`AoEWeapon`, `DashWeapon`, `ProjectileWeapon`)는 개별 공격 로직(`AttackLogic`)을 재정의하여 SRP 충족.
  * **AutoAttack**: 장착된 `List<Weapon>`의 공격 시점 및 글로벌 증강(Modifiers) 수치 동기화를 총괄하여 일괄 배포 전파하는 핵심 허브 컴포넌트.
  * **AudioPooling**: 몬스터 사망 시 객체 내장 오디오 소스가 비활성화되어 사운드가 멈추는 문제를 해결하기 위해, 별도의 독립된 생명주기로 오디오 소스를 재사용하는 Audio Pool 시스템 구축.

### [4단계] Shapes 에셋 및 UI 발전 과정 (Visual Systems)
* **Shapes 에셋 선정 사유**: 3D 모델 및 텍스처 리소스 없이 벡터 그래픽 코드로 사이버펑크 네온 블루 감성의 HDR Glow 효과 구현. 수백 개 타일을 개별 게임 오브젝트 대신 GPU 인스턴싱 단일 드로우콜로 렌더링 최적화.
* **UI 발전 과정**:
  * *Step 1 (단순 스크롤식)*: 스와이프 중심 구조로 정밀 조작 피로도 높음.
  * *Step 2 (순수 클릭식)*: 고정 구조로 가시성 확보는 좋으나 역동성 결여.
  * *Step 3 (하이브리드 UI)*: 스크롤 스냅 Lerp 애니메이션(FocusScale 연출)과 개별 버튼 클릭의 명확성을 통합하여 조작성과 연출 동시 확보.

### [5단계] 기획 - 개발 데이터 연동 (CSV Importer)
* **WeaponID 명명 규칙**: [시트 번호(1~2자)][타입(3~4자)][획득 경로(5~6자)][번호(7~8자)]로 구성하여 데이터 분류 체계 수립.
* **에디터 툴링**: CSV 시트를 ScriptableObject(PureData)로 변환하는 `CSV Importer` 개발. 변환 과정에서 Resources 내 프리팹을 `WeaponID`와 대조하여 자동으로 연동하는 자동 프리팹 맵핑 툴 완성.

### [6단계] 고민과 선택 (Trade-offs)
* **그리드 구조 (RectGrid vs HexGrid)**:
  * 대안 A (RectGrid): 대각선 거리가 가로세로 대비 달라 수학적 물리 거리 일관성 결여.
  * 대안 B (HexGrid): 인접 6개 타일 거리가 동일하여 논리적 일관성이 높고 삼각함수가 불필요한 Cube 좌표 활용이 가능해 최종 채택.
* **UI 상호작용 방식 (스크롤식 vs 클릭식 vs 하이브리드)**:
  * 대안 A (스크롤식): 스와이프 중심이라 피로도 유발.
  * 대안 B (클릭식): 정적 구조로 가시성은 확보되나 역동성 결여.
  * 대안 C (하이브리드 UI): 스냅 Lerp 스케일 동적 연출과 클릭 버튼의 명확성을 결합하여 조작성 및 시각 품질 극대화 목적 최종 채택.

### [7단계] 결과 및 배운 점 (Retrospective)
* **성과**: 1인 3개월 개발 스프린트 통제, GPU 인스턴싱 및 오디오 풀링을 통한 수백 개 적 렌더링 프레임 최적화, CSV 데이터 주도 에디터 툴을 활용한 콘텐츠 확장성 확보.
* **한계 및 개선 조치**:
  * ✓ 달성: DLV 설계 완료, HexGrid 정수 연산 2중 필터링, CSV 자동 바인딩 임포터 구현.
  * △ 한계: 초기 단순 스크롤식 UI의 조작 피로도 및 사망 몬스터 효과음 유실(끊김) 결함 발생.
  * → 해결: 하이브리드 UI(FocusScale 연출) 적용 및 AudioPool Manager 독자 수명주기 설계 완료로 해결.
