## 정의

**둘 이상의 프로세스가 다른 프로세스가 점유하고 있는 자원을 서로 기다릴 때 무한 대기에 빠지는 상황**

![데드락설명](./img/deadlock.png)

## 발생 조건

다음 **4가지 조건**을 모두 만족할 때 발생한다.

1. **상호 배제(Mutual exclusion)**
    - 한 리소스는 한 번에 한 프로세스만이 사용할 수 있음
2. **점유와 대기(Hold and wait)**
    - 어떤 프로세스가 하나 이상의 리소스를 점유하고 있으면서 다른 프로세스가 가지고 있는 리소스를 기다리고 있음
3. **비선점(No preemption)**
    - 프로세스가 태스크를 마친 후 리소스를 자발적으로 반환할 때까지 기다림 (강제로 빼앗지 않는다)
4. **순환(환형) 대기(Circular wait)**
    - 프로세스의 집합에서 순환 형태로 자원을 대기하고 있어야 함

## 방지법

사전에 교착상태가 발생하지 않도록 조치하거나, 발생한 뒤에 고치는 방법이 있다. 대표적으로 아래 **세가지**로 나눈다.

1. **예방(Prevention)**
    
    교착 상태 발생 조건 중 하나를 제거하면서 해결한다.
    
    - 상호배제 부정 : 여러 프로세스가 공유 자원 사용
    - 점유대기 부정 : 프로세스 실행 전 한 번에 모든 자원을 할당
    - 비선점 부정 : 자원 점유 중인 프로세스가 다른 자원을 요구할 때 가진 자원 반납
    - 순환대기 부정 : 모든 자원에 일련의 순서를 부여하고 각 프로세스가 오름차순으로만 자원을 요청할 수 있게 함.
        
    
    이러한 조건을 방지해서 데드락을 예방하는 방법은 **시스템의 처리량이나 효율성을 떨어트리는 단점**이 발생할 수 있다.
    
2. **회피(Avoidance)**
    
    데드락 **회피법**은 예방법보다는 **조금 덜 제한적인 방법**으로 예방법의 단점을 일부 해결할 수 있다.
    
    시스템이 항상 Safe state(안정 상태)에 있을 수 있는 자원만 할당 허용한다. 대표적으로 **은행원 알고리즘**, **자원 할당 그래프**가 있다.
    
    **키워드**
    
    - Safe state(안정 상태)
        
        시스템의 프로세스들이 요청하는 모든 자원을, 데드락을 발생시키지 않으면서도 차례로 모두에게 할당해 줄 수 있다면 **안정 상태**(safe state)에 있다고 한다.
        
    - Safe sequence(안전 순서)
        
        특정한 순서로 프로세스들에게 자원을 할당, 실행 및 종료 등의 작업을 할 때 **데드락이 발생하지 않는 순서를 찾을 수 있다면** 그 순서를 **안전 순서**라고 부른다.
        
    - 불안정 상태
        
        안정 상태가 아닌 상황, 즉 데드락 발생 가능성이 있는 상황이다. 교착상태는 이 상태에서 발생할 수 있다.
        
3. **탐지 및 회복(Detection and Recovery)**
    
    시스템이 예방이나 회피법을 사용하는 대신, 데드락이 발생할 때 그것을 **탐지**하고 **회복**하는 알고리즘을 사용하는 방법
    
    - **탐지 기법**
        - **자원 할당 그래프를 통해 탐지**
        - 자원 요청 시, 탐지 알고리즘을 실행시키므로 그에 대한 오버헤드 발생
    - **회복 기법**
        - 단순히 프로세스를 1개 이상 중단시키기
            - **교착 상태에 빠진 모든 프로세스를 중단시키는 방법**
                
                계속 연산중이던 프로세스들도 모두 일시에 중단되어 부분 결과가 폐기될 수 있는 부작용이 발생할 수 있음.
                
            - **프로세스를 하나씩 중단 시킬 때마다 탐지 알고리즘으로 데드락을 탐지하면서 회복시키는 방법**
                
                매번 탐지 알고리즘을 호출 및 수행해야 하므로 부담이 되는 작업일 수 있음
                
        - 자원 선점하기
            - 교착 상태의 프로세스가 점유하고 있는 자원을 선점해 다른 프로세스에게 할당 (해당 프로세스 일시정지 시킴)
            - 우선 순위가 낮은 프로세스나 수행 횟수 적은 프로세스 위주로 프로세스 자원 선점