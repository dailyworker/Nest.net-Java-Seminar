# 13.4 싱글쓰레드와 멀티쓰레드

## (1) 쓰레드란?
>프로세스에서 프로그램의 흐름을 형성하는것

>한 번에 2개 이상의 흐름으로 프로그램을 진행하고 싶을 때 사용 하는것
## (2) 멀티 쓰레딩의 장점
+ CPU 사용률의 향상
+ 효율적인 자원 관리
+ 응답성 향상
+ 코드의 간결성

# 13.5 쓰레드의 우선순위

## (1) 우선순위 관련 메소드
> 우선순위 설정 메소드: setPriority( 순위 );

> 우선순위 확인 메소드: getPriority( );


# 13.6 쓰레드 그룹 (thread group)

## (1) 쓰레드 그룹이란?
> + 서로 관련된 쓰레드를 그룹으로 다루기 위한것
> + 쓰레드 그룹을 생성해서 쓰레드를 그룹으로 묶어서 관리 할 수 있음
> + 쓰레드 그룹에 다른 쓰레드 그룹을 포함 시킬 수 있음
> + 자신이 속한 쓰레드 그룹이나 하위 쓰레드 그룹은 변경 할 수 있지만, 다른 쓰레드 그룹의 쓰레드를 변경 할 수는 없음
> + ThreadGroup을 사용해서 생성 할 수 있음

## (2) 주요 생성자와 메서드
> ① ThreadGroup(String name) : 지정된 이름의 새로운 쓰레드 그룹을 생성한다
>
> ② ThreadGroup(ThreadGroup parent, String name) : 지정된 쓰레드 그룹에 포함되는 새로운 쓰레드 그룹을 생성한다
>
>③ int activeCount( ) : 쓰레드 그룹에 포함된 활성상태에 있는 쓰레드의 수를 반환한다
>
>④ int activeGroupCount( ) : 쓰레드 그룹에 포함된 활성상태에 있는 쓰레드 그룹의 수를 반환한다
>
>⑤ void checkAccess( ) : 현재 실행중인 쓰레드가 쓰레드 그룹을 변경할 권한이 있는지 체크한다. 만일 권한이 없다면 SecurityException을 발생시킨다
>
>⑥ void destroy( ) : 쓰레드 그룹과 하위 쓰레드 그룹까지 모두 삭제한다. 단, 쓰레드 그룹이나 하위 쓰레드 그룹이 비어있어야한다
>
>⑦ int enumerate(Thread[ ] list) / int enumerate(Thread[ ] list, boolean recurse) /int enumerate(ThreadGroup[ ] list) / enumerate(ThreadGroup[ ] list, boolean recurse) : 쓰레드 그룹에 속한 쓰레드 또는 하위 쓰레드 그룹의 목록을 지정된 배열에 담고 그 개수를 반환한다. 두 번째 매개변수인 recurse의 값을 true로 하면 쓰레드 그룹에 속한 하위 쓰레드 그룹에 쓰레드 또는 쓰레드 그룹까지 배열에 담는다.
>
>⑧ int getMaxPriority( ) : 쓰레드 그룹의 최대우선순위를 반환한다
>
>⑨String getName( ) : 쓰레드 그룹의 이름을 반환한다
>
>⑩ThreadGroup getParent( ) : 쓰레드 그룹의 상위 쓰레드 그룹을 반환한다
>
>⑪void interrupt( ) : 쓰레드 그룹에 속한 모든 쓰레드를 interrupt 한다
>
>⑫boolean isDaemon( ) : 쓰레드 그룹이 데몬 쓰레드그룹인지 확인한다
>
>⑬boolean isDestroyed( ) : 쓰레드 그룹이 사게되었는지 확인한다
>
>⑭void list( ) : 쓰레드 그룹에 속한 쓰레드와 하위 쓰레드 그룹에 대한 정보를 출력한다
>
>⑮boolean parentOf(ThreadGroup g) : 지정된 쓰레드 그룹의 상위 쓰레드그룹인지 확인한다
>
>⑯void setDaemon(boolean daemon) : 쓰레드 그룹을 데몬 쓰레드그룹으로 설정/해제한다
>
>⑰void setMaxPriority(int pri) : 쓰레드 그룹의 최대우선순위를 설정한다


-------------
-------------

13.7 데몬 쓰레드 (daemon thread)
====

일반 쓰레드의 작업을 돕는 보조적인 역할을 하는 쓰레드
----

### 1. **일반 쓰레드가 모두 종료되면 자동으로 종료**됨
### 2. 실행 후 대기하다가 **특정 조건이 만족될 때만 작업 수행**

실행방법
----

### 쓰레드 생성 후 실행하기 전 setDaemon(true) 호출

```java
boolean isDaemon() // 데몬 쓰레드 여부 확인
void setDaemon(boolean on) // 데몬 쓰레드로 변경
```

> **SetDaemon 메서드는 반드시 start()를 호출하기 전에 실행**해야 한다.

- getAllStackTraces()는 작업이 완료되지 않은 모든 쓰레드의 호출스택을 출력

- JVM은 프로그램이 실행되는 데 필요한 데몬 쓰레드들을 자동적으로 생성해서 실행

- GUI를 가진 프로그램을 실행하는 경우에는 이벤트와 그래픽처리를 위해 더 많은 데몬 쓰레드가 생성

13.8 쓰레드의 실행제어
====

쓰레드의 스케줄링과 관련된 메서드
----

| 메서드                                                                  | 설명                                                     |
|-------------------------------------------------------------------------|----------------------------------------------------------|
| static void sleep(long millis) | 지정된 시간동안 쓰레드를 일시정지                        |
| void join(long millis)      | 지정된 시간동안 쓰레드를 실행                            |
| void interrupt()                                                        | sleep(), join()에 의해 일시정지된 쓰레드를 실행대기 상태로 |
| void stop()                                                             | 쓰레드 종료                                              |
| void suspend()                                                          | 쓰레드 일시정지                                          |
| void resume()                                                           | suspend()에 의해 일시정지된 쓰레드를 실행대기 상태로       |
| static void yield()                                                     | 주어진 실행시간 다른 쓰레드에 양보하고 실행대기 상태로   |

쓰레드의 상태
----

| 상태                   | 설명                                                     |
|------------------------|----------------------------------------------------------|
| NEW                    | 쓰레드 생성후 start() 호출되지 않은 상태                 |
| RUNNABLE               | 실행 중 또는 실행 가능한 상태                            |
| BLOCKED                | 동기화블럭에 의한 일시정지 상태                          |
| WAITING, TIMED_WAITING | 작업이 종료되진 않았지만 실행가능하지 않은 일시정지 상태 |
| TERMINATED             | 쓰레드 작업 종료된 상태                                  |

