# 07/22

## 잔디 심기

[Course Schedule II - LeetCode](https://leetcode.com/problems/course-schedule-ii/description/)

## 블로그 정리

[LeetCode 210. Course Schedule II](https://keyone957.tistory.com/36)

[[알고리즘] 위상 정렬](https://keyone957.tistory.com/37)

## cs면접 질문 답변 정리

### 클래스를 구조체로 바꾸었을 때??

<aside>

### 장점

* 성능의 이점

Class ⇒ 참조 타입 따라서 힙 메모리 점유 → 여기저기 흩어진 힙 메모리 참조해야함 따라서 캐시 적중률 낮음
Struct ⇒ 값 타입 따라서 스택 메모리 점유 → 메모리에 연속적으로 배치됨 따라서 캐시 적중률 높음

### 단점

* 복사 비용 비싸

Struct 는 인자로 넘길 때 마다 내부 데이터를 전부 복사함
따라서 데이터가 크면 클 수록 느려짐

* 값 복사에 따른 의도치 않은 버그

값 타입은 복사본이 생성되므로
    void UpdateHealth(MonsterData monster) { 
    // 구조체로 넘김
        monster.hp -= 10; 
        // 이건 복사본을 깎는 것이다!
    }
    // 실제 원본 데이터는 전혀 변하지 않음.

  이와 같은 의도에 맞지 않게 구현될 수 있다

* 구조체는 상속 X 따라서 다형성 구현 X  
  
  </aside>
  
  <aside>
  
  ### 정리
  
  클래스를 구조체로 변경하면 힙 할당과 GC 부담이 줄고 구조체 배열은 메모리에 연속적으로 저장되어 캐시 지역성이 향상되는 장점이 있습니다. 하지만 구조체는 값 타입이므로 대입, 함수 전달, 프로퍼티나 컬렉션 조회 과정에서 값 전체가 복사될 수 있습니다. 이 때문에 복사 비용이 발생하고, 복사본을 수정해도 원본에 반영되지 않는 문제가 생길 수 있습니다.
  
  </aside>
  
  <aside>

* 구조체는 값복사 → 너무 자주 복사되면 성능이 떨어짐

* 수정이 원본에 적용되지 않음. → 값타입이므로  
  
  </aside>

### Delegate & event & UnityEvent

<aside>

### Delegate

> 메서드 참조를 저장하고 실행할 수 있는 타입.

마치 c++에서 함수 포인터와 비슷한 개념

#### 특징

* 멀티 캐스트(델리게이트 체인) 가능
  
  * 하나의 델리게이트 인스턴스가 여러 개의 메서드를 동시에 참조하고, 한 번의 호출로 그 모든 메서드를 순차적으로 실행할 수 있는 기능
    using System;
    class Program
    {
    
        static void TaskA() => Console.WriteLine("A 작업 완료");
        static void TaskB() => Console.WriteLine("B 작업 완료");
        
        static void Main()
        {
            Action myDelegate = TaskA; 
            // TaskA 등록
            myDelegate += TaskB;       
            // TaskB 추가 등록 (이 시점에 멀티캐스트 됨)
        
            myDelegate(); 
            // 실행 시, TaskA와 TaskB가 순서대로 호출됨
        
            myDelegate -= TaskA; 
            // TaskA 제거
            myDelegate();        
            // 이제 TaskB만 호출됨
        }
    
    }
  
  만일 델리게이트 체이닝 중 예외 발생시
  → 연결된 메서드 중 하나라도 예외 발생 시 이후 메서드는 호출 X

</aside>

<aside>

### Event

> Delegate를 안전하게 외부에 공개 하는 문법

* 선언된 클래스 외부에서는 +=.-= 만 가능, 호출 불가

* 이벤트 호출 권한은 선언한 클래스 내부에만 있음

* 안전한 캡슐화를 통해 이벤트 발신자와 수신자를 분리함.  
  
  </aside>
  
  <aside>
  
  ### UnityEvent
  
  > 유니티에서 제공하는 인스펙터에서 연결 가능한 Event
  
  </aside>
  
  <aside>
  
  ### Delegate 와 Event 차이
  
  Delegate는 메서드를 참조하는 타입이며 외부에서 호출과 재할당이 모두 가능합니다.
  Event는 Delegate를 기반으로 하지만 외부에서는 구독과 해지만 가능하고 호출은 선언한 클래스 내부에서만 할 수 있도록 제한하여 캡슐화를 제공합니다. 따라서 옵저버 패턴을 구현할 때는 일반적으로 Event를 사용합니다.
  
  </aside>
  
  <aside>
  
  ### UnityEvent는 잘 안쓰는이유?
  
  유니티 이벤트는 인스펙터에서 연결할 수 있어 UI나 버튼 이벤트에는 매우 편리합니다. 그러나 Reflection과 직렬화 과정으로 인해 일반 Delegate / Event 보다 성능 오버헤드가 있고 코드 추적이 어려워집니다. 그래서 게임 로직은 C# Event를 사용합니다.
  
  </aside>

### 팩토리 패턴 & 추상 팩토리 패턴

<aside>

### 팩토리 패턴

> 객체를 생성하기 위한 인터페이스를 정의하는데, 어떤 클래스의 인스턴스를 만들지는 서브 클래스에서 결정하게 만듦

객체 생성을 팩토리 메서드에 일임
ex)
IMonster monster = monsterFactory.Create(MonsterType.Orc);

</aside>

<aside>

### 추상 팩토리 패턴

> 인터페이스를 이용하여 서로 연관된, 또는 의존하는 객체를 구상 클래스를 지정하지 않고도 생성

몬스터, 무기, 이펙트처럼 서로 관련된 여러 객체를 하나의 제품군으로 묶어 생성
ex) 게임 테마에 맞는 객체들을 한꺼번에 구성함
DesertFactory  
├─ DesertMonster  
├─ DesertWeapon  
└─ DesertEnvironment
SnowFactory  
├─ SnowMonster  
├─ SnowWeapon  
└─ SnowEnvironment

</aside>

<aside>

### 차이점

팩토리 패턴이 주로 하나의 객체 종류를 생성하는 데 초점을 둔다면 추상 팩토리 패턴은 관련된 객체들의 조합과 일관성을 보장하는 데 초점을 둠.



</aside>

### Resources vs Addressables

<aside>

### Resources

> Resources는 `Resources` 폴더에 있는 에셋을 경로(Path)를 이용해 로드하는 Unity의 에셋 관리 방식이다.

* 사용 간단
* 리소스 폴더의 에셋이 빌드에 포함됨
* 메모리 관리와 원격 업데이트에 한계가 있다.
* 로드도 기본적으로 동기 방식이라 프로젝트가 커질수록 빌드 용량과 메모리 관리에 문제가 생김

### Addressables

> 에셋에 주소를 부여하고 필요한 시점에 비동기로 로드, 해제할 수 있는 유니티의 에셋 관리 시스템

* 필요한 에셋만 동적으로 로드 가능

* 비동기 로딩 지원

* 원격 서버에서 에셋 다운로드 및 업데이트 가능  
  
  </aside>

<aside>

### 정리 (차이점)

> Resources는 Resources폴더의 모든 에셋이 빌드에 포함되고 경로 기반으로 로드하는 간단한 방식입니다. 반면 Addressables는 주소 기반으로 필요한 에셋만 비동기로 로드하고 Release()를 통해 메모리를 효율적으로 관리할 수 있습니다.

</aside>


