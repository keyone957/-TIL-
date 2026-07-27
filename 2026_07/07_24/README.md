# 07/24

## 잔디 심기

[Path with Minimum Effort - NeetCode](https://neetcode.io/problems/path-with-minimum-effort/question)



## 면접 스터디 회고



### SetPass Calls

<aside>

### SetPass Calls

> Unity가 GPU에 새로운 렌더링 상태(Shader Pass)를 설정하는 횟수  
> GPU가 다른 셰이더나 메테리얼로 바꾸기 위해 준비 작업을 수행한 횟수

#### SetPass

> Draw Call을 하기 전에 GPU는  
> 어떤 Shader , Material, Texture을 사용할지 등을 먼저 설정해야 하는데 이 과정을 SetPass 라고 함

#### 렌더링 과정

Material 설정 → SetPass → Draw Call → 화면 출력

#### ex 1)

Cube (메테리얼 A) / Sphere (메테리얼 A) / Capsule(메테리얼 A)

* 렌더링 순서

<aside>

메테리얼 A 설정 → SetPass 1회 → Cube Draw 1 → Sphere Draw 2 → Capsule Draw 3  
⇒ SetPass 1회 / Draw Call 3회

</aside>

#### ex 2)

Cube (메테리얼 A) / Sphere (메테리얼 B) / Capsule(메테리얼 C)

<aside>

메테리얼 A → SetPass 1 → Cube Draw 1 → 메테리얼 B → SetPass 2 → Sphere Draw 2 → 메테리얼 C → SetPass 3 → Capsule Draw 3  
⇒ SetPass 3회 / Draw Call 3회

</aside>

</aside>

<aside>

### 정리

> SetPass Call은 GPU가 드로우 콜을 수행하기 전에 Shader, Material 등의 렌더링 상태를 변경하는 횟수이며 렌더링 성능에 큰 영향을 주는 지표이다.

</aside>



### Batches

<aside>

### Batches

> 유니티가 GPU에 제출한 렌더링 명령(드로우 콜)의 개수

</aside>

<aside>

### 정리

> 한 프레임 동안 유니티가 GPU에 보낸 렌더링 명령의 개수. 일반적으로 배치는 하나의 드로우 명령에 해당하며 배치수가 많을 수록 CPU가 GPU에 보내야 하는 명령도 많아져 CPU 부하가 증가합니다.

</aside>



### SetPass Call 최적화 방식

<aside>

> 셋 패스 콜은 렌더링 중 쉐이더, 메테리얼, 텍스쳐 렌더 상태가 변경될 때 발생하는 상태 변경 횟수다 . 따라서 핵심은 렌더링 상태 변경을 줄이는 것

### 1. Material 수 줄이기

> 같은 쉐이더를 사용하더라도 메테리얼 인스턴스가 다르면 SetPass가 증가할 수 있음 가능하면 동일한 메테리얼을 공유하자

### 2. 같은 Shader와 Material 공유

> 여러 오브젝트가 같은 쉐이더와 메테리얼을 사용하도록 구성하면 셋 패스콜 줄어듦

### 3. Texture Atlas 사용

> 여러 텍스쳐를 하나의 Atlas로 합치면 메테리얼을 통합하기 쉬워진다. 결과적으로 하나의 아틀라스를 사용하면 텍스쳐, 메테리얼 변경이 준다

### 4. SRP Batcher 활성화

> URP나 HDRP 환경에서 사용할 수 있는 설정임. 같은 쉐이더를 사용하는 메테리얼 사이의 CPU 상태 변경 비용을 줄여준다.

다만 이는 셋 패스 콜을 크게 줄이는 기술이라기 보다 SetPass와 관련된 CPU비용을 줄이는 기술에 가까움

</aside>



### 트라이

<aside>

### 정의

> GPU가 한 프레임 동안 렌더링 하는 삼각형의 개수

3D 그래픽에서 모든 모델은 사각형이나 원으로 그려지는 것이 아닌 삼각형으로 분할되어 렌더링된다.
EX) 큐브 하나도 실제로는 6개의 면이 각각 2개의 삼각형로 이루어져 있어 12개의 삼각형으로 구성된다.

</aside>

<aside>

### 정리

> 트라이는 gpu가 한 프레임 동안 처리하는 삼각형의 개수입니다. 3d 모델은 모두 트라이앵글로 구성되기 때문에 tris 수가 많을 수록 GPU가 처리해야하는 연산이 증가합니다. 따라서 TRIS 수가 과도하면 GPU 부하가 커져 FPS가 저하될 수 있으며 이를 줄이기 위해 LOD, 로우 폴리 , 오클루젼 컬링 등을 적용해 최적화할 수 있습니다

</aside>



### Vertex (정점)

<aside>

### Vertex

> ⇒ 메쉬를 구성하는 가장 기본적인 단위의 점 각 정점은 3D 공간에서의 좌표를 가지고 있고 다양한 정보가 포함되어있음

#### Vertex 정보

* Position : Vertex의 위치 좌표
* Normal : 빛의 반사 방향을 계산하기 위한 법선 벡터
* UV : 텍스처의 어느 부분을 사용할지 결정하는 좌표
* Color : 정점 색상  
  
  </aside>

<aside>

### 정리

> Vertex는 3D 모델을 구성하는 가장 기본적인 정점입니다. 위치뿐 아니라 Normal, UV, Color, Bone Weight 등 렌더링에 필요한 다양한 정보를 가지고 있습니다. GPU는 Vertex Shader에서 먼저 Vertex를 처리한 뒤 이를 연결해 Triangle을 생성하고 화면을 렌더링합니다. 따라서 Vertex 수가 많을수록 Vertex Shader의 연산량이 증가하여 GPU 부하가 커질 수 있으며, LOD나 Low Poly 모델 등을 통해 최적화할 수 있습니다.

</aside>



### FPS

<aside>

### 정의

> 1초 동안 화면에 출력되는 프레임의 개수 → 1초에 화면이 몇 번 새로 그려지는 지를 나타내는 성능 지표

### FPS가 높으면 항상 좋은 것인가?

> FPS가 높으면 일반적으로 화면이 부드럽고 입력 지연이 줄어들지만, 항상 높은 것이 최선은 아닙니다. 디스플레이 주사율을 크게 초과하면 체감 이득은 제한적이고 전력 소비와 발열이 증가할 수 있습니다.

</aside>



### Scriptable 과 Monobehaviour과 다른점

* Monobehaviour
  * 게임 오브젝트에 컴포넌트로 붙어서 동작하는 게임 로직 담당
  * 생명 주기 메서드를 가짐
  * transform 같은 게임 오브젝트에 접근할 수 있는 속성을 가짐
* Scriptable Object
  * 게임 데이터를 파일 형태로 저장하고 공유함
  * 생명 주기 메서드 없음, 게임 오브젝트에 종속되지 않음, 에셋파일로 존재 (경량화)
  * 여러 게임 오브젝트나 스크립트가 하나의 데이터 인스턴스를 공유할 수 있음


