# 데드락이란?

> 두 개 이상의 프로세스나 스레드가 서로 자원을 얻지 못해서 다음 처리를 하지 못하는 상태
무한히 다음 자원을 기다리게 되는 상태를 말한다.
- 한정된 자원을 여러 곳에서 사용하려고 할 때 발생

![](https://velog.velcdn.com/images/ririti/post/3ef31e87-e342-4a2c-8fed-3b12e44ffd3c/image.png)

process1과 process2이 각각 할당되어 있는 자원을 서로가 사용중인 자원을 얻기 위해
기다리는데 이 때 두 프로세스는 무한정 wait 상태에 빠지게 된다.
-> 이를 deadlock이라 부른다.

## 데드락의 발생 4가지 발생 조건
> 4가지 모두 성립되어야 한다.

- **상호 배제**
    - 한 자원은 한번에 한 프로세스만 사용할 수 있다.

- **점유 대기**
    - 최소한 하나의 자원을 점유하고 있으면서 다른 프로세스에 할당되어 사용하고 있는 자원을 추가로 점유하기 위해 대기하는 프로세스가 존재

- **비선점**
    - 다른 프로세스에 할당된 자원은 사용이 끝날 때까지 강제로 빼앗을 수 없다.

- **순환대기**
    - 대기 프로세스의 집합이 순환 형태로 자원을 대기하고 있어야 한다.

# 데드락의 3가지 해결책
- 데드락이 발생하지 않도록 예방
- 데드락의 발생 가능성이 있음을 인지하고 회피
- 데드락 발생이 허용하지만 데드락을 탐지하여 데드락에서 회복

## 데드락 예방
> 데드락의 발생조건 4가지 중 하나라도 발생하지 않게 하는 것이 데드락을 예방하는 방법
> - 시스템의 처리량이나 효율성을 떨어트리는 단점 - (자원을 많이 낭비함)

- **자원의 상호 배제 조건 방지**: 여러 프로세스가 공유 자원을 사용
    - 추후 동기화 관련 문제가 발생할 수 도있다.

- **점유대기 조건 방지**: 프로세스 실행에 필요한 모든 자원을 한꺼번에 요구하고 허용할 때 까지 작업을 보류

- **비선점 조건 방지**: 다른 프로세스에게 할당된 자원이 선점권이 없다고 가정할 때, 높은 우선순위의 프로세스가 해당 자원을 선점할 수 있도록 한다.

- **순환 대기 조건 방지** : 자원을 순환 형태로 대기하지 않도록 일정한 한 쪽 방향으로만 자원을 요구할 수 있도록 한다.

## 데드락 회피

**Safe state**: 시스템의 프로세스들이 요청하는 모든 자원을, 데드락을 발생시키지 않으면서도 차례로 모두에게 할당해 줄 수 있다면 안정 상태(safe state)라 한다.

**Safe state**: 특정한 순서로 프로세스들에게 자원을 할당, 실행 및 종료 등의 작업을 할 때 데드락이 발생하지 않는 순서를 찾을 수 있다면, 그것을 안전 순서(safe sequence)라 한다.

이와 달리 불안정 상태는 안정 상태가 아닌 데드락 발생 가능성이 있는 상황을 의미하며 교착상태는 불안정 상태일 때 발생하게 된다.

> 데드락 회피 알고리즘은 자원을 할당한 후에도 시스템이 항상 안정상태에 있을 수 있도록 할당을 허용하는것이 기본 특징

### 은행원 알고리즘
> 어떤 자원의 할당을 허용하는지에 관한 여부를 결정하기 전에, 미리 결정된 모든 자원들의 최대 가능한 할당량을 가지고 시뮬레이션 해서 Safe state에 들 수 있는지 여부를 검사
> - 대기중이 다른 프로세스들의 활동에 대한 교착 상태 가능성을 미리 조사하는 것


* 은행원 알고리즘의 경우 미리 최대 자원 요구량을 알아야하며 할당할 수 있는 자원 수가 일정해야하는 제약조건같은 단점이 있다.

## 데드락 탐지 및 회복

- **탐지 기법**
    - Allocation, Request, Available 등으로 시스템에 데드락이 발생했는지 여부를 탐색
    - 자원 할당 그래프를 통해 탐지하는 방법도 있다.

- **회복 기법**
  탐지 기법을 통해 데드락을 발견했다면 순환 대기에서 벗어나 데드락으로부터 회복해야한다.

    - **단순히 프로세스를 한개 이상 중단**

        - 교착 상태에 빠진 모든 프로세스를 중단시키는 방법: 연산중이던 프로세스들도 모두 중단되어 결과물이 제대로 도출 안돼는 부작용이 발생 할 수 있다.
        - 교착 상태가 제거될 때까지 한 프로세스씩 중지 : 매번 탐지 알고리즘을 호출하고 찾아야 하므로 부담이 되는 작업

    - **자원 선점하는 방법**
        - 교착 상태의 프로세스가 점유하고 있는 자원을 선점하여 다른 프로세스에게 할당하며, 해당 프로세스를 일시 정지 시키는 방법
        - 우선 순위가 낮은 프로세스, 수행된 횟수가 적은 프로세스 등을 위주로 프로세스의 자원을 선점