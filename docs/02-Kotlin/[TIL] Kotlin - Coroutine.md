---
layout: default
title: "[TIL] Kotlin - Coroutine"
date: 2024-05-16 +0800
last_modified_at: 2024-05-16 +0800
tags: [TIL Kotlin]
categories:
  - TIL
toc: true
parent: Kotlin
---

# [TIL] Kotlin - Coroutine

## **Coroutine...?**<br><br>

>💡 **Coroutine**<br>
    코루틴(coroutine)은 루틴의 일종으로서, 협동 루틴이라 할 수 있다(코루틴의 "Co"는 with 또는 together를 뜻한다). 상호 연계 프로그램을 일컫는다고도 표현가능하다.<br><br>
    - [위키백과](https://ko.wikipedia.org/wiki/%EC%BD%94%EB%A3%A8%ED%8B%B4)

<br>

**Coroutine이란?**<br>
실행의 지연과 재개를 허용함으로써, `비선점적 멀티 태스킹`을 위한 서브루틴을 일반화한 컴퓨터 프로그래밍 구성요소를 말한다.<br>
코루틴은 병행성(동시성)은 제공, 병렬성은 제공하지 않는다.<br>

{: .note }
> **비선점적 멀티 태스킹...?** <br><br>
`비선점적 멀티태스킹(Coroutine)`: 하나의 태스크가 다른 태스크가 실행 중이어도 프로세서(CPU)를 차지 할 수 있다.<br>
`선점적 멀티태스킹(Thread)`: 우선 순위(상대적 중요도, 자원 소모량, 기타 다른 요인)가 부여되는 태스크, 하나의 태스크가 실행 중이라면 다른 태스크가 프로세서(CPU)를 차지 할 수 없다.

<br>


**Routine이란?**<br>
- 하나의 Task, Fuction을 말한다.
- Main Routine과 Sub Routine으로 나뉜다. Main Routine이 Sub Routine을 호출하는 방식을 취한다.
- 코루틴의 경우 Main, Sub 개념을 구분하지 않는다. 모든 Routine들이 서로를 호출할 수 있다.
- 진입과 탈출이 자유롭다. Sub Routine은 return을 만나야만 탈출한다.
    - 닫는 괄호를 만나면 해당 Sub Routine을 빠져 나온다.
    - return문을 만나면 Sub Routine을 호출했던 부분으로 탈출한다.

<br>

---

## **Coroutine의 기본 개념**<br>
Coroutine을 이해하기 위해서는 3개의 기본 개념을 알아햐 한다.<br><br>
1. **Coroutine Scope**: Coroutine이 실행되는 영역(MainScope, GlobalScope, CoroutineScope 등)
2. **Coroutine Builders**: Coroutine을 생성하는 메소드(launch, async, withContext, runBlocking 등)
3. **Coroutine Context**: key와 element를 갖는 map.(element에는 서브타입으로 job, deferred, dispatcher등) Coroutine Context를 이용하면 Coroutine 취소와 같은 작업을 간단히 처리 할 수 있다.

<br>

---
## **Suspend Function**<br>
코루틴을 사용하게 된다면 상당히 많이 접하게 될 키워드가 suspend 키워드다.<br>
함수 앞에 suspend라는 변경자를 붙여 사용한다.<br>
함수에 필요한 모든 런타임 문맥을 저장하고 함수 실행을 중단한 다음, 나중에 필요할 때 다시 계속 진행할 수 있게 해준다.<br>

```kotlin
suspend fun task() {
    println("시작")
    delay(1000)
    println("종료")
}
```

{: .note }
> **delay()...?** <br>
coroutine 라이브러리에 정의된 일시 중단 함수<br>
Thread.sleep()과 비슷한 일을 한다. 일시 중단 함수를 호출하면 해당 호출 지점이 일시 중단 지점이 되며 일시 중단 지점은 나중에 재개할 수 있는 지점이 된다.

<br>

---
## **Callback과 Coroutine의 가독성**<br>
Callback은 백그라운드 스레드를 사용해 긴 작업을 실행하고, 백그라운드 스레드에서 작업이 완료되면 메인 스레드에 정의된 콜백을 호출하여 작업 결과를 알려주는 방법입니다.<br>
Callback은 Main-Safe하게 작업을 처리 할 수 있지만, 반복적인 들여쓰기로 인해 가독성이 떨어집니다.(~~콜백지옥~~)

```kotlin
// 순차적인 네트워크 호출을 나타내는 코드

fun getGingerBrave(api: CookieService): gingerBrave {
    api.makeDough{ dough -> 
        api.addMagicPowder(dough){ -> magicDough
            api.escapeOven(magicDough) { -> cookie
                api.fetchGingerBrave(cookie) { -> gingerBrave
                    Log.d("You can't catch me! I'm the Gingerbre.. I'm Ginger Brave!")
                    return gingerBrave
                }
            }
        }
    }
}
```

```kotlin
// Coroutine을 이용해 변경한 코드

suspend fun getGingerBrave(api: CookieService): gingerBrave {
    val dough = api.makeDough()
    val magicDough = api.addMagicPowder(dough)
    val cookie = api.escapeOven(magicDough)
    val gingerBrave = api.fetchGingerBrave(cookie)
    Log.d("You can't catch me! I'm the Gingerbre.. I'm Ginger Brave!")
    return gingerBrave
}
```

Callback과 Coroutine은 Main-Safe한 작업을 하게 해준다는 공통점을 가지고 있지만, Coroutine은 콜백 기반 코드를 순차적 코드(sequential code)로 나타낼 수 있기 때문에 비동기 코드를 단순화할 수 있습니다.<br>
이는 기본적으로 suspend 키워드는 내부적으로 Callback을 생성하기에 가능합니다.
<br>

{: .note }
> **Main-Safe** <br>
메인스레드가 제 역할을 하도록 Blocking하지 않는 것을 의미한다.<br>
Android는 Single-Thread 모델로 UI작업은 메인스레드(UI스레드)에서만 실행될 수 있다.<br>
UI는 사용자와 직접적으로 맞닿는 부분으로 앱 응답성과 직결되므로 중요도가 높은 작업이다. 따라서 메인스레드가 항상 이 역할을 수행할 수 있도록 앱을 설계해야 한다.

<br>

---
## **Coroutine Builders**<br>
코루틴 빌더는 Coroutine Scope 인스턴스의 확장 함수로 쓰인다.<br>
Coroutine Scope에 대한 구현 중 가장 기본적인 것으로 GlobalScope 객체가 있으며, 이를 사용하면 독립적인 Coroutine을 만들 수 있다.

1. **launch**
    - 현재 스레드를 중단하지 않고 코루틴을 바로 시작한다.
    - Coroutine을 시작하고, Coroutine을 실행 중인 작업의 상태를 추적하고 변경할 수 있는 Job 객체를 돌려준다.
    - CoroutineScope.() -> Unit 타입의 일시 중단 람다를 받는다.
    - 동시성 적업이 결과를 만들어 내지 않는 경우에 사용된다. 따라서 이 빌더는 Unit 타입을 반환하는 람다를 인자로 받는다.

```kotlin
import kotlinx.coroutines.*

fun main() {
    val time = System.currentTimeMillis()

    GlobalScope.launch {
        delay(100)
        println("Task 1 finished in ${System.currentTimeMillis() - time}")
    }

    GlobalScope.launch {
        delay(100)
        println("Task 2 finished in ${System.currentTimeMillis() - time}")
    }

    //코틀린을 처리하는 스레드는 데몬 모드로 실행되기 때문에 메인 스레드가 종료되면 실행이 종료되는 것을 막기 위해 Thread.sleep(200) 사용
    Thread.sleep(200)
}

// 실행결과
Task 1 finished in 197
Task 2 finished in 197
```

2. **async**
    - 현재 스레드를 중단하지 않고 Coroutine을 바로 시작한다. await()을 통해 Coroutine 결과를 기다릴 수 있다.
    - Coroutine을 시작하고, Defferred의 인스턴스를 돌려준다. 이 인스턴스는 Job의 하위 타입으로 await 메서드롤 통해 계산 결과에 접근할 수 있게 해준다. await 메서드를 호출하면 계산이 완료되거나 계산 작업이 취소 될 때까지 현재 Coroutine을 일시 중단 시킨다.

```kotlin
import kotlinx.coroutines.*

suspend fun main() {
    val message = GlobalScope.async {
        delay(100)
        "hello "
    }

    val count = GlobalScope.async {
        delay(100)
        3
    }

    delay(100)

    val result = message.await().repeat(count.await())
    println(result)
}

// 실행 결과
hello hello hello
```

3. **runBlocking**
    - launch와 async 빌더의 경우 스레드 호출을 blocking 시키지는 않고, 백그라운드 스레드 풀을 통해 작업을 실행한다.
    - runBlocking() 빌더는 현재 스레드에서 실행되는 Coroutine을 만들고 Coroutine이 완료될 때까지 현재 스레드의 실행을 블락 시킨다.
    - Coroutine이 종료되면 일시 중단 람다의 결과가 호출의 결과값이 된다.
    - runBlocking() 내부의 Coroutine은 메인 스레드에서 실행되고 launch()로 시작한 Coroutine은 공유 풀에서 백그라운드 스레드를 할당 받는다. 이런 블로킹 동작 때문에 runBlocking()은 다른 Coroutine 안에서 사용하면 안된다. runBlocking()은 블로킹 호출과 넌 블로킹 호출 사이의 다리 역할을 하기 위해 고안된 코루틴 빌더이므로, 테스트나 메인 함수에서 최상위 빌더로 사용하는 등의 경우에만 runBlocking()을 써야 한다.
    - 메인 스레드에서 runBlocking을 사용하여 스레드를 장시간 점유하고 있을 경우 ANR(Application Not Responding)이 발생할 수 있다.

```kotlin
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch {
        delay(100)
        println("백그라운드 스레드 작업: ${Thread.currentThread().name}")
    }

   runBlocking {
       println("메인 스레드 작업: ${Thread.currentThread().name}")
       delay(100)
   }
}

// 실행 결과
메인 스레드 작업: main
백그라운드 스레드 작업: DefaultDispatcher-worker-1
```

4. **withContext**
    - 부모 코루틴에 의해 사용되던 context와 다른 context에서 코루틴을 실행 할 수 있다.
    - async()와 동일한 역할을 하는 키워드, 결과값을 반환하는 형태
    - async()는 결과값을 얻으려면 await()을 호출해야 하지만 withContext는 처음부터 결과 리턴까지 대기하는 형태

<br>

---

## **Coroutine Scope**<br>
1. **GlobalScope**: 앱 프로세스의 생명 주기를 따라감
    - Application이 시작하고 종료될 때까지 계속 유지가 된다.
    - Singletone이기 때문에 따로 생성하지 않아도 되며 어디에서든 바로 접근이 가능하여 간단하게 사용하기 쉽다.
2. **MainScope**: UI 관련 작업을 처리하는 용도.
3. **ViewModelScope**: ViewModel의 생성주기를 따라감
4. **LifeCycleScope**: Activity, Fragment의 생명주기를 따라감. 생명주기별로 콜백이 다르다.
5. **coroutineScope**: 다수의 코루틴을 suspend 함수가 시작하고 모든 코루틴이 완료될 때만 어떤 처리가 필요한 경우에 적합하다. 여러 코루틴 중에서 하나라도 실패하면 모든 코루틴이 취소된다.
6. **supervisorScope**: coroutineScope와 비슷하지만 여러 코루틴 중에서 하나가 실패해도 다른 코루틴이 취소되지 않는다는 점이 다르다.

<br>

### coroutineScope
- 가장 기본적인 CoroutineScope
- 지정된 컨텍스트에 Job 요소가 없으면 기본 Job()이 생성 (Job을 설정하지 않을 시 디폴트로 생성이 된다는 의미)
- 스코프 안에 다른 스코프가 존재할 때 부모를 캔슬 하면 자연스럽게 안에 자식도 캔슬된다. 부모에 영향이 미치면 자식도 영향을 받게 된다.

```kotlin
val job = Job()
CoroutineScope(Dispatchers.IO + job).launch{ code 1 }
CoroutineScope(Diapatchers.IO + job).launch{ code 2 }

// 변수로 job을 설정하면 다중 Job 설정이 가능해진다.
// job.cancel을 했을 때 두 스코프 모두 영향을 받게 된다.
```

### LifecycleScope
- lifecycle 소유자의 lifecycle과 연계되어 있다.
- 라이프 사이클이 destoryed되면 이 스코프가 취소된다.
- Dispatchers.Main.immediate에 바인딩 되어 있다.
- lifecycleScope.launch(Dispatchers.IO) {} 해당 스코프를 Activity에서 생성하면 자동으로 Activity의 라이프 사이클과 연계된다.
### ViewModelScope
- ViewModel에 연결된 CoroutineScope
- ViewModel이 지워지면(ViewModel.onCleared가 호출) 해당 스코프가 취소된다.
- Dispatchers.Main.immediate에 바인딩 되어 있다.
- ViewModel에서만 사용할 수 있다.
- ViewModel의 라이프사이클과 연관되어 있다.
- ViewModel은 결국 Activity 라이프사이클을 따라가기 때문에 앱을 종료하면 중단된다.

{: .note }
> **Dispatchers.Main.immediate** <br>
Dispatchers.Main과 Dispatchers.Main.immediate는 다르다.<br>
Dispatchers.Main은 순서를 보장하지 않고 Dispatchers.Main.immediate는 순서를 보장해준다.

<br>

---
## **Dispatchers의 종류**<br>
1. **Dispatchers.Default**: 안드로이드 기본 스레드 풀을 사용한다. CPU를 많이 쓰는 데이터 정렬, 복잡한 연산 작업에 최적화
2. **Dispatchers.IO**: 이미지 다운로드, 파일 입출력 등 네트워크, 디스크, DB 작업에 최적화
3. **Dispatchers.Main**: 안드로이드 기본 스레드에서 코루틴을 실핸한다. UI와 상호작용에 최적화
4. **Dispatchers.Unconfined**: GlobalScope와 함께 사용, 코루틴이 현재 스레드에서 작동한다. 중단되고 다시 시작되면 중단된 스레드에서 시작. 안드로이드 앱 개발에서는 사용하지 않는 것을 권고한다.

<br>

---
## **Continuation**<br>
- 컴퓨터 프로그램의 제어 상태를 추상적으로 표현한 것. 프로세스 실행의 주어진 지점에서 계산해야 할 프로세스를 나타내는 데이터 구조
- 다음에 수행해야 할 작업을 구조적으로 나타낸 것
- kotlin에서는 Continuation 객체를 통해 다음에 수행할 중단/재개 시점을 Context에 저장하여 관리한다. Continuation 객체는 Callback Interface를 일반화한 객체라고도 볼 수 있다.