XR Lab
3D 팀 프로젝트
김우태	Name
rladnxo3577@naver.com	Mail
010-6471-8024	Phone

-- 1 of 36 --

목차
프로젝트 개요	01 	03 - 07	P
시스템 요소	02 	08 - 25	P
미니 게임	03 	26 - 35	P

-- 2 of 36 --

프로젝트 정보
01	프로젝트 개요
04
프로젝트 정보 	팀원 소개
해당 프로젝트는 보드게임 치킨 차차를 온라인 멀티 게임으로
이식하는 팀 프로젝트 입니다.
개발 인원 : 5명
개발 기간 : 2025.05.21 ~ 2025.05.30 (총 17일)
프로젝트 개요
프로젝트 개요	01 	04	P
팀원 소개	02 	05	P
01
시스템 요소	03 	06	P
미니 게임 목록	04 	07	P

-- 3 of 36 --

01	프로젝트 개요
04
프로젝트 정보 	팀원 소개
프로젝트 정보
해당 프로젝트는 Unity에서 제작한 라이브러리
XR Interaction Toolkit을 사용해 XR에 대한 기술을 학습하고
미니 게임을 만드는 것이 목적인 프로젝트입니다.
개발 인원 : 2명
개발 기간 : 2025.06.09 - 2025.06.27 (총 18일)
이 문서는 XR Interaction Toolkit의 기본 기능 설명과
기본 기능을 사용해 개발 한 미니 게임 설명 순으로 진행됩니다.

-- 4 of 36 --

01	프로젝트 개요
05
팀원 소개 	시스템 요소
팀원 소개 	팀 프로젝트를 함께 진행한 팀원입니다.

-- 5 of 36 --

시스템 요소 XR Interaction Toolkit을 사용해 학습한 시스템과 시각 효과를 위해 제작한 Phase, Dissolve
효과 입니다.
01	프로젝트 개요
06
시스템 요소 	미니 게임 목록

-- 6 of 36 --

미니 게임 목록 시스템 요소들을 활용해 제작한 미니 게임입니다. 해당 문서에서는 제가 제작한 사격 게임과
피규어 룸에 대해서 설명합니다.
01	프로젝트 개요
07
미니 게임 목록

-- 7 of 36 --

챕터 개요팀 단위 작업이 처음이었던 팀원들과 원활히 협업하기 위해서는 개발 환경을 통일하는 것부터
해야한다고 생각했습니다. 개발 환경 통일을 위해 아래와 같은 절차를 진행 했습니다.
가독성을 우선한 코드 컨밴션 작성
코딩 스탠다드에 기반한 Rider IDE 환경설정
환경 설정의 공유
개발 환경 설정
02	기술 리드
07
개발 환경 설정 - 코드 컨밴션
상호 작용	01 	09 - 14	P
프로젝트 주요 컴포넌트	02 	15 - 21	P
시스템 요소	02
시각 효과	03 	22 - 25	P

-- 8 of 36 --

챕터 개요 XR Interaction Toolkit에서 가상 환경 내 객체와 상호 작용하는 핵심 요소인 Grab Interactor와
Interactable 컴포넌트에 대해 설명합니다.
챕터 개요
02	Grab & Trigger
12
Trigger
챕터 개요	01 	10	P
Interactor	02 	11	P
Interactable	03 	12	P
상호 작용
Interaction Layer	04 	13	P
Interaction Event	05 	14	P

-- 9 of 36 --

챕터 개요 XR Interaction Toolkit에서 가상 환경 내 객체와 상호 작용하는 핵심 요소인 Interactor와
Interactable 컴포넌트에 대해 설명합니다.
챕터 개요
02	상호 작용
10
Interactor

-- 10 of 36 --

Interactor
02
11
Interactable
Interactor Interactor는 컨트롤러에 부착되어 사용자의 상호 작용을 감지하고 처리하는 컴포넌트입니다.
컴포넌트 종류별로 물체 인식 방법이 다릅니다. 이는 주로 사용자의 손을 나타내는 게임 오브젝트에 추가됩니다.
상호 작용

-- 11 of 36 --

Interactable
02
12
Interaction Layer
Interactable Interactable은 사용자가 상호 작용할 수 있는 가상 오브젝트에 부착되는 컴포넌트입니다. 예를
들어, 잡거나 던질 수 있는 물체, 누를 수 있는 버튼 등에 사용됩니다.
상호 작용

-- 12 of 36 --

Interaction Layer
02
13
Trigger - Interaction Event
Interaction Layer InteractionLayer는 Interactor와 Interactable 간의 상호 작용을 필터링하고 제어하는 데 사용됩니다.
동일한 InteractionLayer에 속한 Interactor와 Interactable만 서로 상호 작용할 수 있습니다.
상호 작용

-- 13 of 36 --

Trigger - Interaction Event
02
14
Trigger Interaction Event 설정을 통해 잡고 있는 물체에 대한 특정 트리거 행동(예: 버튼 클릭, 특정 동작 수행)
을 결정하고 사용자 정의할 수 있습니다.
상호 작용

-- 14 of 36 --

챕터 개요 XR Interaction Toolkit에서 가상 환경 내 객체와 상호 작용하는 핵심 요소인 Grab Interactor와
Interactable 컴포넌트에 대해 설명합니다.
챕터 개요
02	Grab & Trigger
12
Trigger
챕터 개요	01 	16	P
Grab Interactor	02 	17	P
프로젝트 주요 컴포넌트
Socket Interactor	03 	18 - 19	P
Lever	04 	20	P
Lock Grid Socket	05 	21	P

-- 15 of 36 --

챕터 개요 본 프로젝트에서 사용된 주요 Interactor에 대해 소개합니다.
Grab Interactor와 SocketInteractor가 있습니다.
챕터 개요
02	프로젝트 주요 컴포넌트
12
Grab Interactor

-- 16 of 36 --

Grab Interactor Grab Interactor는 컨트롤러에 부착되어 사용자의 "잡기" 동작을 수행하는 컴포넌트입니다.
이는 주로 사용자의 손을 나타내는 게임 오브젝트에 추가됩니다.
Grab Interactor
02	프로젝트 주요 컴포넌트
12
Socket Interactor

-- 17 of 36 --

Socket
Interactor
Socket Interactor는 사용자가 Interactable 오브젝트를 특정 슬롯이나 소켓 위치에 배치하는 동작을
수행하는 컴포넌트입니다. 이는 주로 물체를 삽입하거나 거치할 슬롯/소켓 역할을 하는 게임 오브젝트에
추가됩니다.
Socket Interactor
02	프로젝트 주요 컴포넌트
12
Socket Interactor - 사용 예시

-- 18 of 36 --

Socket
Interactor
총기 케이스(Socket) 안에 총기(Interactable Obejct)가 들어가 있는 좌측 이미지,
총기를 총기 케이스에서 꺼낸 우측 이미지 입니다.
Socket Interactor - 사용 예시
02	프로젝트 주요 컴포넌트
12
Lever를 응용한 총기 케이스

-- 19 of 36 --

Lever XR Interaction Toolkit의 Lever기능을 응용해 총기 케이스의 열고 닫는 기능을 구현 헀습니다.
좌측 이미지 닫힌 모습과, 우측 이미지 열린 모습입니다.
Lever를 응용한 총기 케이스
02	프로젝트 주요 컴포넌트
12
Lock Grid Socket

-- 20 of 36 --

Lock Grid
Socket
Socket Interactor의 기능을 확장한 기능으로, Interaction Layer이외에도 Key값을 검사합니다.
알맞은 Key값을 가진 물체를 대상으로 동작합니다. 그 결과 같은 레이어 다른 상호작용이 가능합니다.
Lock Grid Socket
02	프로젝트 주요 컴포넌트
12

-- 21 of 36 --

챕터 개요 XR Interaction Toolkit에서 가상 환경 내 객체와 상호 작용하는 핵심 요소인 Grab Interactor와
Interactable 컴포넌트에 대해 설명합니다.
챕터 개요
02	Grab & Trigger
12
Trigger
챕터 개요	01 	23	P
Phase	02 	24	P
Dissolve	03 	25	P
시각 효과

-- 22 of 36 --

챕터 개요 XR이라는 프로젝트 컨셉에 맞춘 시각 효과를 위해 Shader Graph로 제작한 Phase효과(좌측 이미지)
Dissolve효과(우측 이미지)에 대해 이야기합니다.
Phase 효과	챕터 개요
02	시각 효과
23

-- 23 of 36 --

Phase Shader Graph로 작업한 Phase효과 이미지 입니다. 해당 프로젝트에서는
작은 소품들 (예 : 스프레이 캔)을 생성할 때 사용합니다.
Dissolve 효과	Phase 효과
02	시각 효과
24

-- 24 of 36 --

Dissolve Shader Graph로 작업한 Dissolve효과 이미지 입니다. 해당 프로젝트에서는
플레이어가 위치한 룸(방)을 전환할 때 사용합니다.
Interactor	Dissolve 효과
02	시각 효과
25

-- 25 of 36 --

챕터 개요팀 단위 작업이 처음이었던 팀원들과 원활히 협업하기 위해서는 개발 환경을 통일하는 것부터
해야한다고 생각했습니다. 개발 환경 통일을 위해 아래와 같은 절차를 진행 했습니다.
가독성을 우선한 코드 컨밴션 작성
코딩 스탠다드에 기반한 Rider IDE 환경설정
환경 설정의 공유
개발 환경 설정
02	기술 리드
07
개발 환경 설정 - 코드 컨밴션
사격 게임	01 	27 - 29	P
피규어 룸	02 	30 - 35	P
미니 게임	03

-- 26 of 36 --

챕터 개요 XR Interaction Toolkit에서 가상 환경 내 객체와 상호 작용하는 핵심 요소인 Grab Interactor와
Interactable 컴포넌트에 대해 설명합니다.
챕터 개요
02	Grab & Trigger
12
Trigger
챕터 개요	01 	28	P
게임 진행 흐름	02 	29	P
사격 게임

-- 27 of 36 --

사격 게임 흐름도
챕터 개요 	앞서 설명한 기능들로 구현한 사격 게임에 대해 설명 합니다.
챕터 개요
02	사격 게임
28

-- 28 of 36 --

Interactor	게임 흐름도
02	사격 게임
29
게임 흐름 	Lever로 구현된 총기 케이스를 열고 Grab Interactor로 총을 집으면 게임이 시작됩니다.

-- 29 of 36 --

챕터 개요 XR Interaction Toolkit에서 가상 환경 내 객체와 상호 작용하는 핵심 요소인 Grab Interactor와
Interactable 컴포넌트에 대해 설명합니다.
챕터 개요
02	Grab & Trigger
12
Trigger
챕터 개요	01 	31	P
진행 흐름	02 	32	P
피규어 룸
스프레이 생성	03 	33	P
피규어 도색	04 	34	P
피규어 진열장	04 	34	P

-- 30 of 36 --

챕터 개요 	앞서 설명한 기능들로 구현한 피규어 룸 입니다.
진행 흐름도	챕터 개요
02	피규어 룸
31

-- 31 of 36 --

스프레이 생성	진행 흐름도
02	피규어 룸
32
게임 흐름 	색상을 선택해서 스프레이로 생성하고 생성한 스프레이로 피규어를 도색합니다.

-- 32 of 36 --

피규어 도색	스프레이 생성
02	피규어 룸
33
색상 선택 	ColorPicker를 통해 색상을 선택하고 해당 색상으로 스프레이를 만들 수 있습니다.

-- 33 of 36 --

피규어 진열장	피규어 도색
02	피규어 룸
34
피규어 도색 	생성한 스프레이를 사용해서 피규어를 도색할 수 있습니다.

-- 34 of 36 --

피규어 진열장
02	피규어 룸
35
진열장 	진열장 모델에 Lock Socket을 사용해 진열장을 구현했습니다. 피규어를 진열할 수 있습니다.

-- 35 of 36 --

THANK YOU
감사합니다

-- 36 of 36 --

