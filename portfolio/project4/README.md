# 뱀서라이크 (Vampire Survivors Like) 기술 사양서

**Unity (Unity 6 / 6000.3.8f1) · Shapes (Vector Graphics) · C# · 1인 개인 프로젝트**

본 문서는 1인 개발 환경에서 고품질 벡터 그래픽스 최적화를 달성하기 위해 'Shapes' 에셋을 활용하고, 대규모 물량 처리를 위한 3축 Cube 육각형 좌표계(Hex-Grid) 및 데이터-로직-비주얼(DLV) 격리 아키텍처를 설계하여 구현한 개인 프로젝트의 종합 기술 명세서입니다.

---

## 1. 프로젝트 개요

* **목표**: 몬스터 스폰 물량이 기하급수적으로 증가하는 상황에서 연산 병목을 제거하고, 기획 사양의 빠른 콘텐츠 추가가 가능하도록 데이터 중심 프레임워크를 정립합니다.
* **개발 환경**: Unity 6000.3.8f1, Sourcetree, Shapes Vector Graphics Asset
* **장르적 본질**: '성장의 축적'과 '물량의 쾌감' 극대화.
* **3대 설계 지표**:
  * **확장성**: 무기와 증강 데이터를 코드 수정 없이 순수 데이터 시트(PureData) 편집만으로 무한 확장되도록 분리.
  * **가시성**: 육각형 그리드 가이드를 활용해 복잡한 대난투 상황에서도 공격 사거리와 안전 구역을 실시간 직관 인지.
  * **연속성**: 단판 세션 종료 후 획득 포인트로 메타 계정 성장(해금) 옵션을 유기적 연결.

---

## 2. 장르 분석 및 기획

### 2.1. 조작 및 게임 루프
* **단순한 조작**: 플레이어는 캐릭터 이동 및 회피 연산에만 100% 시선을 집중하며, 무기는 장착된 쿨타임 주기 및 탐색 로직에 맞춰 자동으로 조작 없이 사출됩니다.
* **성장 피드백 순환 구조**:
  $$\text{이동/조작} \longrightarrow \text{적 처치} \longrightarrow \text{경험치/샤드 수집} \longrightarrow \text{레벨업/무기 선택} \longrightarrow \text{캐릭터 강화}$$
  세션이 완전히 종료되면 획득한 누적 결과 데이터에 비례해 메타 포인트를 정산하고, 영구적 계정 옵션을 해금합니다.

### 2.2. 중앙 집중식 UX 디자인 및 비주얼 테마
* **중앙 집중 UX**: 수백 마리 몬스터 회피 시 플레이어 시선이 화면 구석의 체력/경험치 바(UI)로 향할 경우, 조작 실수를 유발합니다. 이를 방어하기 위해 모든 시각적 피드백을 캐릭터 중심으로 수렴시켰습니다.
  * **내부 원형 게이지**: 플레이어 캐릭터 바로 밑에 % 비례 체력을 원형으로 드로잉 표시.
  * **외부 원형 테두리 게이지**: 캐릭터 외곽 테두리를 감싸며 % 비례 경험치(Exp) 실시간 드로잉 표시.
* **비주얼 가이드 테마 Swatches**:
  * **네온 블루**: 플레이어 캐릭터의 영역, 이로운 아이템, 아군 공격 범위 및 획득 경험치 정보 표출.
  * **네온 레드**: 적 몬스터의 충돌 범위, 위협 투사체 궤적 및 몬스터 장착 공격 범위 표출. 아군과 적의 연출 색상을 완벽히 분리해 시각 가독성을 확보했습니다.

---

## 3. Data - Logic - Visual (DLV) 아키텍처

관심사 분리 원칙을 기반으로 데이터, 연산 로직, 시각 렌더링 처리를 3단계로 완벽히 격리한 DLV 패턴을 적용했습니다.

### 3.1. Data (데이터 보존 및 실시간 구독)
* **PureData**: 기획 수치 및 사양이 기재된 불변(Immutable) 설계도입니다.
  * **규칙**: 모든 필드는 `readonly` 혹은 `private set`으로 선언되어 생성 이후 수정이 불가능하며, 데이터를 변경/검증하는 로직을 일절 포함하지 않는 순수 데이터 구조(Logic-Free) 규격을 강제합니다. (ScriptableObject, CSV 파싱 데이터 구조체)
* **PureDataBase**: 생성된 `PureData` 목록을 `Dictionary<string, PureData>` 테이블 형태로 통합 관리하며, 훼손되지 않도록 보호합니다.
* **RuntimeData**: 런타임 게임 진행 중 동적으로 연동되어 변동되는 데이터 모델입니다.
  * **규칙**: `PureData`를 읽기 전용으로 참조(Reference)하며, 변경 시 C# 이벤트를 발송하는 Observable 인터페이스를 구현하고, 자체 `Tick()` 메서드로 자신의 수명을 스스로 갱신합니다.

### 3.2. Logic (의사결정 및 상황 판단)
* **역할**: `RuntimeData` 값을 연산 비교하여 상황을 판단하고 `Visual`에 행동 명령을 내립니다.
* **규칙**: 유니티 `Rigidbody`, `Collider`, `NavMeshAgent` 등 렌더링 및 물리 기하 컴포넌트를 직접 참조하거나 수정하지 않습니다. 심지어 `transform.position` 값조차 직접 변경하지 않고, 물리/이동 처리가 필요하면 추상화된 `Visual` 인터페이스에 "이동하라"고 위임 명령만 수행합니다.

### 3.3. Visual & Object Visual (DIP 의존성 역전)
* **역할**: 구체적인 렌더링 표현 방식을 캡슐화하는 핵심 레이어입니다.
* **규칙**: `ObjectVisual` 추상 인터페이스 형태(`MoveTo()`, `Stop()`, `Attack()`)로 선언되며, 내부에 2D Shapes 벡터 그래픽을 쓰는지 3D 폴리곤 메쉬를 쓰는지, 혹은 NavMesh를 타는지 일반 Translate 변환을 하는지 등의 구체적 구현 방법을 상위 로직에 절대 노출하지 않습니다. (DIP 실현)
* **UI Visual**: 로직을 경유하지 않고 `RuntimeData` 이벤트를 직접 옵저버 패턴으로 구독(Subscribe)하여 슬라이더 및 텍스트 게이지를 Canvas에 실시간 동기화합니다.

---

## 4. Shapes 에셋과 HexGrid 기술 구현

### 4.1. Shapes 에셋을 활용한 벡터 그래픽 및 드로우콜 최적화
* **선정 사유**: 리소스가 한정된 1인 개발 환경에서 고퀄리티 비주얼을 수학적 코드로 직접 생성합니다. 매끄러운 벡터 라인과 빛나는 HDR Glow 네온 라인을 제공해 가시성을 극대화합니다.
* **드로우콜 단축**: 수백 개 타일을 일반 유니티 Game Object로 생성 시 오버헤드가 매우 큽니다. Shapes의 **GPU 인스턴싱(GPU Instancing)** 드로잉 기법을 사용해 하나의 배치 드로우콜로 통합 렌더링하여 프레임 드랍을 원천 차단했습니다.

### 4.2. 3축 Cube 육각형 좌표계 (HexGrid) 설계
사각형 격자는 인접 타일과의 물리적 거리가 방향별($1$ 대 $\sqrt{2}$)로 불일치하여 다방향 거리 연산 시 논리적 모순이 발생합니다. 주변 6개 타일로의 물리 거리가 균일한 Hex-Grid를 채택하고, 삼각함수 연산 없이 정수만으로 격자 거리를 도출하기 위해 3축 ($q + r + s = 0$) Cube 좌표 시스템을 구축했습니다.

```csharp
// 2D 배열 인덱스(col, row)를 3축 Cube 좌표계(Vector3Int)로 즉시 전환
private Vector3Int OffsetToCube(int col, int row)
{
    var q = col - (row - (row & 1)) / 2;
    var r = row;
    return new Vector3Int(q, -q - r, r); // q + r + s = 0을 연산식에서 강제
}

// 3D 월드 상의 연속 좌표를 가장 근접한 정수형 육각 셀 좌표로 Rounding 매핑
public Vector3Int WorldToCube(Vector3 worldPos)
{
    float width = Mathf.Sqrt(3f) * hexRadius;
    float height = 2f * hexRadius * 0.75f;

    // 지그재그 홀수/짝수 행 보정 처리
    int row = Mathf.RoundToInt(worldPos.z / height);
    float xOffset = (row % 2 != 0) ? width * 0.5f : 0f;
    int col = Mathf.RoundToInt((worldPos.x - xOffset) / width);

    return OffsetToCube(col, row);
}
```

* **맨해튼 거리 공식 (GetHexDistance)**:
  제곱근(`Mathf.Sqrt`) 연산은 코스트가 높습니다. 3축 좌표계의 정수 가감산 나눗셈 공식을 통해 런타임 계산 속도를 연산 장치 한계까지 극대화했습니다.
  $$\text{Distance} = \frac{|q_1 - q_2| + |r_1 - r_2| + |s_1 - s_2|}{2}$$
  ```csharp
  public int GetHexDistance(Vector3Int a, Vector3Int b)
  {
      return (Mathf.Abs(a.x - b.x) + Mathf.Abs(a.y - b.y) + Mathf.Abs(a.z - b.z)) / 2;
  }
  ```

### 4.3. 2중 범위 탐색 최적화 (OverlapHexGrid)
가상 물리 구체 검사(`Physics.OverlapSphere`)와 육각 정수 거리 판정 알고리즘을 2단계 파이프라인으로 구축해 다중 타겟 선별을 가속했습니다.

```csharp
// 물리 검출 후보 수집 ➡️ 정밀 타일 거리 필터링의 2중 탐색 루틴
public List<Collider> ScanTargets(Vector3 centerPos, int range, LayerMask targetLayer)
{
    List<Collider> validTargets = new List<Collider>();
    
    // 1단계: Physics.OverlapSphere로 가상의 넓은 원형 물리 영역 타겟 수집 (가볍고 빠름)
    float hexWidth = Mathf.Sqrt(3f) * hexRadius;
    float searchRadius = (hexWidth * range) + hexWidth;
    Collider[] hits = Physics.OverlapSphere(centerPos, searchRadius, targetLayer);
    
    Vector3Int centerCube = WorldToCube(centerPos);
    
    // 2단계: 수집된 후보군을 순회하며 육각 큐브 거리가 유효 범위(range) 내에 있는지 정밀 대조
    foreach (var hit in hits)
    {
        Vector3Int hitCube = WorldToCube(hit.transform.position);
        if (GetHexDistance(centerCube, hitCube) <= range)
        {
            validTargets.Add(hit); // 범위 필터링 완료
        }
    }
    return validTargets;
}
```

---

## 5. 인게임 사이클 및 무기(Weapon) 프레임워크

### 5.1. 다형성 기반 Weapon 추상화 설계
모든 고유 무기가 공유하는 공통 제어기(쿨타임, 스탯 연산, 활성 상태 등)는 부모 `Weapon` 추상 클래스에 정의하여 코드 중복을 제거하고, 세부 공격 패턴만 자식 클래스가 구현하도록 단일 상속 구조를 성립했습니다.

```csharp
// Weapon.cs - 무기 추상화 골격
public abstract class Weapon : MonoBehaviour
{
    protected RuntimeDataWeapon model; // 실시간 스탯 반영 모델
    protected IWeaponVisualizer visuals; // 시각 연출 추상 인터페이스

    public abstract void WeaponSettingLogic(); // 고유 설정부
    
    public virtual void AttackLogic()
    {
        visuals.PlayAttackAnimation(); // 공통 비주얼 연출
        model.SetCooldown(model.FinalAttackDelay); // 공통 쿨타임 처리
    }
}
```

### 5.2. WeaponModifier 계층 스탯 연산
무기의 원본 SO 데이터(`PureData`)를 직접 변경하지 않고, 인게임 증강 선택 수치(`Local`)와 전역 스택 효과(`Global`)를 실시간으로 결합해 최종 전투 스탯(`RecalculateStats`)을 무결성 검증 산출합니다.

```csharp
private void RecalculateStats()
{
    if (PureData == null) return;

    // 공격 속도(Speed) 역산하여 딜레이값 도출 (최소 0.05초 보장 방어 코드)
    float totalSpeedMultiplier = Mathf.Max(0.1f, m_attackDelayMultiplier + m_globalAttackDelayModifier);
    FinalAttackDelay = Mathf.Max(0.05f, PureData.AttackDelay / totalSpeedMultiplier);

    // 가산 수치와 승산 배율의 계층적 연산 처리
    FinalDamage = (int)((PureData.Damage + m_damageAdded + m_globalDamageAdded) * 
                  (m_damageMultiplier + m_globalDamageMultiplier));

    FinalEffectRange = PureData.EffectRange * m_effectRangeMultiplier;
    FinalProjectileCount = PureData.ProjectileCount;

    // 연산 즉시 옵저버 이벤트 퍼블리싱하여 UI 갱신 자동화
    OnStatsChanged?.Invoke();
}
```

### 5.3. 무기 다형성 세부 종류
* **AoEWeapon (범위 지속 무기)**:
  정해진 주기마다 `ScanTargets()`를 호출하여 본인 주변 육각 그리드 내 몬스터들에 즉각적인 전역 데미지를 입히는 형태의 공격 로직 탑재.
* **ChargeDashWeapon (돌진형 몬스터 전용 무기)**:
  일반 투사체와 달리 몬스터 본체 자체를 임시 충돌체(Collider)로 활성화하고 목표 방향으로 직접 물리 돌진 돌격 연산. 공격 속도가 돌진 기동 성능과 직접 비례 연계.
* **ProjectileWeapon (오브젝트 풀링 투사체 무기)**:
  `FinalProjectileCount` 스탯 개수 비례로 투사체를 순차 사출. 매 공격마다 Instantiate/Destroy를 호출하지 않고, **오브젝트 풀링(Object Pooling)**에서 비활성 오브젝트를 재사용하여 프레임 드랍 완벽 방어.

---

## 6. 기획 - 개발 연결 흐름 및 최적화

### 6.1. 데이터 시트 5종 규격
기획 사양에 따라 데이터 시트를 5가지 영역으로 모듈화하여 관리합니다.
1. `StatAugment` (캐릭터 기본 스탯 증강 시트)
2. `WeaponAugment` (무기 전용 업그레이드 수치 시트)
3. `MonsterDatas` (몬스터 종별 체력, 속도, 드롭 경험치 시트)
4. `StageData` (스테이지별 웨이브 타임라인, 스폰 룰 시트)
5. `WeaponData` (무기 쿨타임, 투사체 속도, 데미지 원천 시트)

### 6.2. WeaponID 명명 규칙 테이블
원천 데이터 시트의 식별자(ID) 정합성을 위해 8자리 고유 정수 키값을 코드에서 다음과 같이 파싱 처리합니다.
| 자릿수 구분 | 용도 설명 | 실제 파싱 예시 (`05010002` 기준) |
| :--- | :--- | :--- |
| **1~2자리** | 원본 데이터 시트 분류 번호 | `05` (5번 무기 시트 소속) |
| **3~4자리** | 무기 고유 메커니즘 분류 타입 | `01` (광역 AoE 타입 분류) |
| **5~6자리** | 무기 입수 및 해금 경로 | `00` (기본 해금 상태) |
| **7~8자리** | 그룹 내 개별 일련번호 | `02` (2번째 뱀서 무기 개체) |

* **자동 리소스 연결**: 유니티 에디터 스크립트 기반 **CSV Importer**를 통해 작성한 Excel/Notion CSV 데이터 시트를 에디터 단에서 버튼 클릭 한 번으로 `ScriptableObject` 에셋 파일로 임포트하며, 파일 적재 시 `WeaponID`와 유니티 Resources 폴더 내의 프리팹 ID명을 대조 대입해 물리 프리팹을 자동으로 에셋 데이터에 동적 매핑하는 자동화 연결망 구축.

### 6.3. AudioPooling 최적화 명세
* **문제 상황**: 몬스터 사망 효과음(피격음)이 몬스터 프리팹 내부에 강결합되어 부착된 구조일 때, 몬스터 체력이 0이 되어 `SetActive(false)`로 즉시 풀링 반납되는 찰나에 오디오 컴포넌트까지 동시 비활성화되어 효과음이 툭 끊기며 타격감을 해치는 병목 발생.
* **해결 방안**: 오디오 재생 책임을 몬스터 객체에서 격리 해제. 별도의 전용 **AudioPool Manager**를 설계하여, 몬스터 피격/사망 트리거 발생 시 오디오 풀러에서 비활성 오디오 소스 오브젝트를 팝(Pop)하여 월드 위치에 독립 배치 후 클립 길이만큼 완전 재생 후 스스로 반납하는 라이프 사이클 보존 아키텍처 구축.
