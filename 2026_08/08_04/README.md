08/04
=====

잔디 심기
-----

[Longest Increasing Path in a Matrix - LeetCode](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/description/)

## 블로그 정리

[LeetCode 329. Longest Increasing Path in a Matrix](https://keyone957.tistory.com/45)

## VR 프로젝트

- MCP 환경 구축 에러 수정 및 환경 구축 완

- 퍼즐 매니저

- 

## 면접 스터디 질문 준비

CS 공부
-----



### [응집도 VS 결합도]

<aside>

#### 응집도

> 클래스나 모듈 내부의 기능들이 **하나의 목적을 위해 얼마나 잘 모여 있는지**를 의미

#### 응집도가 낮은 경우

    class PlayerManager
    {
        public void MovePlayer() { }
        public void SaveGame() { }
        public void PlayBgm() { }
        public void SendNetworkPacket() { }
    }

  ⇒ 서로 관련성이 낮은 기능들이 섞여 있음

  하나의 클래스가 여러 책임을 담당함.

#### 응집도가 높은 경우

      class PlayerMovement
      {
          public void Move() { }
          public void Jump() { }
          public void Dash() { }
      }

  ⇒ 모두 플레이어 이동과 관련된 기능들이 모여 있음

#### 응집도가 높을 때 장점

* 클래스의 역할을 이해하기 쉬움

* 수정해야하는 위치를 찾기 쉬움

* 코드의 재사용성이 높아짐

* 한 기능이 다른 기능에 미치는 영향이 줄어듦
  ⇒ 하나의 클래스가 하나의 책임만 담당하면 일반적오르 응집도가 높아짐 ⇒ **`SRP`**
  
  </aside>
  
  <aside>
  
  #### 결합도
  
  > 결합도는 클래스나 모듈 사이의 **의존 관계가 얼마나 강한지**를 의미
  
  #### 결합도가 높은 경우
  
      class Player
      {
        private FireSkill fireSkill = new FireSkill();
      
        public void UseSkill()
        {
            fireSkill.Execute();
        }
      }
  
  ⇒ player가 fireball이라는 구체적인 클래스를 직접 생성하고 사용
  
      class Player
      {
          private IceSkill iceSkill = new IceSkill();
      
          public void UseSkill()
          {
              iceSkill.Execute();
          }
      }
  
  ⇒ 만일 스킬을 변경하려면 player코드도 위와 같이 수정해야함.
  → player가 구체적인 스킬 구현에 강하게 의존 따라서 결합도 높음

#### 결합도가 낮은 경우

      interface ISkill
      {
          void Execute();
      }
    
      class IceSkill : ISkill
      {
          public void Execute()
          {
              // 얼음 스킬 실행
          }
      }
      class Player
      {
          private ISkill skill;
    
          public Player(ISkill skill)
          {
              this.skill = skill;
          }
    
          public void UseSkill()
          {
              skill.Execute();
          }
      }

  ⇒ player는 여러 스킬을 직접 알 필요 없고 ISkill 인터페이스에만 의존

</aside>

### [좋은 설계가 높은 응집도와 낮은 결합도를 지향하는 이유]

<aside>

> 높은 응집도는 하나의 모듈이 하나의 책임에 집중하도록 하여 이해와 수정, 테스트를 쉽게 만듦.  
> 낮은 결합도는 모듈 간의 의존성을 줄여 한 부분의 변경이 다른 부분으로 전파되는 것을 방지  
> ⇒ 높은 응집도와 낮은 결합도를 가진 설계는 유지보수성, 확장성, 재사용성, 테스트 용이성이 높다

### [클래스와 구조체를 선택하는 기준]

      class PlayerClass
      {
          public int Hp;
      }
    
      struct PlayerStruct
      {
          public int Hp;
      }

#### 클래스

  PlayerClass a = new PlayerClass { Hp = 100 };  
  PlayerClass b = a;

  b.Hp = 50;

  // a.Hp도 50

> 참조 형식 → 클래스 변수에는 객체에 대한 참조가 저장

* 객체의 크기가 크다

* 상태가 자주 변경된다

* 여러 곳에서 동일한 객체를 공유해야한다.

* 상속과 다형성이 필요하다

* 객체의 생명 주기를 관리해야한다.  

#### 구조체

  PlayerStruct a = new PlayerStruct { Hp = 100 };  
  PlayerStruct b = a;
  b.Hp = 50;
  // a.Hp는 여전히 100

> 값 타입 → 구조체를 대입 a와 b는 독립적

* 크기가 작다.

* 하나의 단순한 값을 표현함

* 상속이 필요 없다

* 생성된 이후 변경되지 않는 불변 객체로 사용할 수 있다.  
  
  

### [중간 삽입이 많다는 이유 만으로 연결 리스트를 선택하면 안되는 이유?]

> 연결 리스트의 중간 삽입이 `O(1)`인 것은 **삽입할 노드를 이미 알고 있을 때**만 해당합니다. 위치를 매번 탐색해야 한다면 전체 비용은 `O(n)`이며, 연결 리스트는 노드가 메모리에 흩어져 있어 **캐시 효율도 낮고** **노드별 추가 메모리와 할당 비용도 발생**합니다. 반면 배열의 원소 이동은 연속 메모리 복사로 빠르게 처리될 수 있습니다.  
> 따라서 단순히 중간 삽입이 많다는 이유가 아니라, 노드 참조를 즉시 얻을 수 있고 실제 측정에서 원소 이동이 병목일 때 연결 리스트를 선택해야 합니다.
