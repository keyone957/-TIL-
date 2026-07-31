# 07/31

잔디 심기
-----

[Letter Combinations of a Phone Number - NeetCode](https://neetcode.io/problems/combinations-of-a-phone-number/question)

[Matchsticks to Square - NeetCode](https://neetcode.io/problems/matchsticks-to-square/question)



## 블로그 작성

[LeetCode 473. Matchsticks to Square](https://keyone957.tistory.com/43)



## VR 플젝

- 여동생 컴포넌트 기능 추가

- 페이즈 연결

- 



## CS 공부

### [컨텍스트란? Context?]

<aside>

#### Context

> 프로세스 또는 스레드가 실행을 이어가기 위해 필요한 CPU의 실행 상태 정보

#### 포함되어있는 정보

* CPU 레지스터 값

* 프로그램 카운터 PC

* 스택 포인터 SP

* 프로세스 상태  
  
  </aside>

<aside>

#### 컨텍스트 스위칭

> 현재 실행 중인 프로세스 OR 스레드의 컨텍스트를 저장하고 다음에 실행할 프로세스의 컨텍스트를 복원하는 과정

예시)
프로세스 A
         ↓

  A의 실행 상태 (Context) 저장
           ↓

  프로세스 B
          ↓

  B의 실행 상태(Context) 복원

</aside>

### 

### [async / await]

<aside>

> async / awiat  
> ⇒ 비동기 프로그래밍을 위한 키워드

### async

> 비동기 메서드를 선언할 때 사용하는 키워드

### await

> 비동기 메서드 내에서 사용되는 키워드

⇒ 해당 비동기 작업이 완료될 때까지 현재 메서드의 실행을 일시 중지시키며 비동기 작업이 완료되면 메서드의 실행을 계속 진행함.



</aside>

### [async / await 와 코루틴 적절한 사용처]

<aside>

#### async / await

> 비동기 작업을 순차적인 코드처럼 작성할 수 있도록 하는 C#의 비동기 프로그래밍 기능

* Task 기반으로 동작 ⇒ I/O 작업이나 시간이 오래 걸리는 비동기 작업을 효율적으로 처리 가능

* async / await 를 사용했다고 해서 자동으로 새로운 스레드가 생성되는 것은 아님!
  ex) `Task.Delay`, 파일 입출력, 네트워크 통신 등은 비동기적으로 처리되며, CPU 연산을 별도 스레드에서 실행하려면 `Task.Run()` 등을 사용해야 한다.
1. 반환값 지원
   코루틴은 값을 반환하기 번거로움. 그러나 async / await는 쉬움

2. 예외 처리
   코루틴은 내부에서 `try - catch` 로 에러를 잡기 힘들지만 async / await 는 에러 캐치가 가능

3. 병렬 제어
   Task.WhenAll 등을 사용해 비동기 작업을 동시에 쏴놓고 한번에 기다리는 등의 고급 통제가 훨 쉬움

#### Coroutine

> 유니티에서 여러 프레임에 걸쳐 작업을 수행하기 위한 기능

  메인 스레드에서 실행되고 별도의 스레드를 생성 하지 않는다.

</aside>

<aside>

#### Async/Await를 사용하는 것이 좋은 경우

> 비동기 작업이나 오래 걸리는 작업에 적합

* 서버 통신

* REST API 호출

* 파일 저장 및 로드

* 데이터베이스 접근
  await File.ReadAllTextAsync(path);  
  처럼 파일을 읽는 동안 메인 스레드를 막지 않는다.
  
  

#### Coroutine을 사용하는 것이 좋은 경우

> **프레임 단위로 동작해야 하는 작업**에 적합

* 일정 시간 대기

* UI 애니메이션

* 페이드 인 아웃
