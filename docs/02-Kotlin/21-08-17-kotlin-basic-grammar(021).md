---
layout: default
title: "[Kotlin] 코틀린 기초 모든 클래스가 정의해야 하는 메소드"
date: 2021-08-17 +0800
last_modified_at: 2021-08-17 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(021) 

## <span style="color:orange">1. 모든 클래스가 정의해야 하는 메소드</span>  
---  

```kotlin
// 예제에 사용할 Client 클래스의 초기 정의
class Client(val name: String, val postalCode: Int)
```

<br><br>

## <span style="color:orange">문자열 표현 toString()</span>
---  

```kotlin
//Client에 toString() 구현하기
class Client(val name: String, val postalcode: Int) {
    override fun toString() = "Client(name=$name, postalCode=$postalCode)"
}

val client1 = Client("홍길동", 4122)
println(client1)
// 결과 = Client(name=홍길동, 4122)
```

이런 문자열 표현으로부터 기본 문자열 표현보다 더 많은 정보를 얻을 수 있다.

<br><br>

## <span style="color:orange">객체의 동등성: equals()</span>
---  

Client 클래스를 사용하는 모든 계산은 클래스 밖에서 이뤄진다. Client는 단지 데이터를 저장할 뿐이며, 그에 따라 구조도 단순하고 내부 정보를 투명하게 외부에 노출하게 설계됐다. 그렇지만 클래스는 단순할지라도 동작에 대한 몇가지 요구 사항이 있을 수 있다. 예를 들어 서로 다른 두 객체가 내뷍 동일한 데이터를 포함하는 경우 그 둘을 동등한 객체로 간주해야 할 수도 있다.

```kotlin
val client1 = Client("홍길동", 4122)
val client2 = Client("홍길동", 4122)

// 코틀린에서 == 연산자는 참조 동일성을 검사하지 않고 객체의 동등성을 검사한다.
// 따라서 == 연산은 equals를 호출하는 식으로 컴파일된다.
println(client1 == client2)

// 결과 = false
```

```kotlin
// Client에 equals() 구현하기
class Client(val name: String, val postalCode: Int) {
    // Any는 java.lang.Object에 대응하는 클래스, 코틀린의 최상위 클래스
    override fun equals(other: Any?): Boolean {
        // other가 Client인지 검사한다.
        if (other == null || other !is Client)
            return false
        // 두 객체의 프로퍼티 값이 서로 같은지 검사한다.
        return name == other.name && postalCode == other.postalCode
    }
}
```
코틀린의 is 검사는 자바의 instanceof와 같다. is는 어떤 값의 타입을 검사한다.  
equals를 오버라이드하고 나면 프로퍼티의 값이 모두 같은 두 고객 객체는 동등하다 예상할 수 있다. 실제로 이제는 true를 반환한다. 하지만 Client 클래스로 더 복잡한 작업을 수행해보면 제대로 작동하지 않는 경우가 있다. 이 경우에는 hashCode가 없다는 점이 원인이다.

<br><br>

## <span style="color:orange">해시 컨테이너: hashCode()</span>
---  

자바에서는 equals를 오버라이드할 때 반드시 hashCode도 함께 오버라이드해야 한다.
```kotlin
val processed = hashSetOf(Client("홍길동", 4122))
println(processed.contains(Client("홍길동", 4122)))
// 결과 = false
```

이는 Client 클래스가 hashCode 메소드를 정의하지 않았기 때문이다. JVM 언어에서는 hashCode가 지켜야 하는 "equals()가 true를 반환하는 두 객체는 반드시 같은 hashCode()를 반환해야 한다."라는 제약이 있는데 Client는 이를 어기고 있다.  
이 문제를 고치려면 Client가 hashCode를 구현해야 한다.

```kotlin
// Client에 hashCode 구현하기
class Client(val name: String, val postalCode: Int) {
    override fun hashCode() : Int = name.hashCode() * 31 + postalCode
}
```

---

<br>

*참고 서적 : Kotlin IN Action*