---
layout: default
title: "[Kotlin] 코틀린 기초 클래스 위임"
date: 2021-08-20 +0800
last_modified_at: 2021-08-20 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(023) 

## <span style="color:orange">1. 클래스 위임: by 키워드 사용</span>  
---  

대규모 객체지향 시스템을 설계할 때 시스템을 취약하게 만드는 문제는 보통 구현 상속에 의해 발생한다.  
코틀린에서는 이런 문제를 인식하고 기본적으로 클래스를 final로 취급하기로 결정했다. 상속을 염두에 두고 open 변경자로 열어둔 클래스만 확장할 수 있다.  
하지만 종종 상속을 허용하지 않는 클래스에 새로운 동작을 추가해야 할 때가 있다. 이럴 때 사용하는 일반적인 방법이 _데코레이터(Decorator)_패턴이다. 이 패턴의 핵심은 상속을 허용하지 않는 클래스 대신 사용할 수 있는 새로운 클래스를 만들되 기존 클래스와 같은 인터페이스를 데코레이터가 제공하게 만들고, 기존 클래스를 데코레이터 내부에 필드로 유지하는 것이다. 이때 새로 정의해야 하는 기능은 데코레이터의 메소드에 새로 정의하고(기존 클래스의 메소드나 필드 활용 가능) 기존 기능이 그대로 필요한 부분은 데코에이터의 메소드가 기존 클래스의 메소드에세 요청을 전달한다.  
단점은 준비 코드가 상당히 많이 필요하다는 점이다.

```kotlin
// 데코레이터 생성 예시
class DelegatingCollection<T> : Collection<T> {
    private val innerList = arrayListOf<T>()

    override val size : Int get() = innerList.size
    override fun isEmpty() : Boolean = innerList.isEmpty()
    override fun contains(element: T) : Boolean = innerList.contains(element)
    override fun iterator() : Iterator<T> = innerList.iterator()
    override fun containsAll (elements: Collection<T>) : Boolean = 
        innerList.containsAll (elements)
}
```

이런 위임을 언어가 제공하는 일급 시만 기능으로 지원한다는 점이 코틀린의 장점이다.  
by 키워드를 통해 해당 인터페이스에 대한 구현을 다른 객체에 위임 중이라는 사실을 명시할 수 있다.

```kotlin
// by 키워드를 사용한 데코레이터 생성 예시 변경
class DelegatingCollection<T> (
    innerList: Collection<T> = ArrayList<T>()
) : Collection<T> by innerList {}
```

클래스 안에 있던 모든 메소드 정의가 없어졌다. 컴파일러가 그런 전달 메소드를 자동으로 생성한다.  
메소드 중 일부의 동작을 변경하고 싶은 경우 메소드를 오버라이드하면 컴파일러가 생성한 메소드 대신 오버라이드한 메소드가 쓰인다. 기존 클래스의 메소드에 위임하는 기본 구현으로 충분한 메소드는 따로 오버라이드할 필요가 없다.  

```kotlin
// 클래스 위임 사용하기
// 원소를 추가하려고 시도한 횟수를 기록하는 컬렉션의 구현
class CountingSet<T> (
    val innerSet: MutableCollection<T> = HashSet<T> ()
) : MutableCollection<T> by innerSet {
    var objectsAdded = 0

    override fun add(element: T): Boolean {
        objectsAdded++
        return innerSet.add(element)
    }

    override fun addAll (c: Collection<T>): Boolean {
        objectAdded += c.size
        return innerSet.addAll(c)
    }
}

val cset = CountingSet<Int>()
cset.addAll(listOf(1, 1, 2))
println("${cset.objectAdded} objects were added, ${cset.size} remain")
// 결과 = 3 object were added, 2remain
```

add와 addAll을 오버라이드해서 카운터를 증가시키고, MutableCollection 인터페이스의 나머지 메소드는 내부 컨테이너(innerSet)에게 위임한다.  
이때 CountingSet에 MutableCollection의 구현 방식에 대한 의존관계가 생기지 않는다는 점이 중요하다.

---

<br>

*참고 서적 : Kotlin IN Action*