---
layout: default
title: "[Kotlin] 코틀린 기초 컬렉션 처리: 가변 길이 인자, 중위 함수 호출, 라이브러리 지원"
date: 2021-07-28 +0800
last_modified_at: 2021-07-28 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(009) 

## <span style="color:orange">1. 자바 컬렉션 API 확장</span>  
---  
```kotlin
val strings: List<String> = listOf("first", "second", "fourteenth")
strings.last()
// 결과값 = fourteenth

val numbers: Collection<Int> = setOf(1, 14, 2)
numbers.max()
// 결과값 = 14
```

last와 max는 확장 함수다.  
코틀린 표준 라이브러리는 수많은 확장 함수를 포함하고 있다.  
IDE의 코드 완성 기능을 통해 함수들을 살펴보고 원하는 함수를 선택해 사용할 수 있도록 하자.

<br><br>

## <span style="color:orange">2. 가변 인자 함수: 인자의 개수가 달라질 수 있는 함수 정의</span>
---
```kotlin
//이미 배열에 들어있는 원소를 가변 길이 인자로 넘기는 코드
fun main(args: Array<String>) {
    val list = listOf("args : ", *args)
    println(list)
}
```
자바에서는 배열을 그냥 넘기면 되지만 코틀린에서는 배열을 명시적으로 풀어서 배열의 각 원소가 인자로 전달되게 해야한다. 전달하려는 배열 앞에 *을 붙이면 된다.  

<br><br>

## <span style="color:orange">3. 값의 쌍 다루기: 중위 호출과 구조 분해 선언</span>
---
```kotlin
//map 만들기
val map = mapOf(1 to "one", 7 to "seven", 53 to "fifty-three")
```
이 코드는 중위 호출이라는 방식으로 to라는 일반 메소드를 호출한 것이다.  
중위 호출 시에는 수신 객체와 유일한 메소드 인자 사이에 메소드 이름을 넣는다.  이때 객체, 메소드 이름, 유일한 인자 사이에는 공백이 들어가야 한다.

```kotlin
//아래 두 호출은 동일하다.
1.to("one")
1 to "one"
```

인자가 하나뿐인 일반 메소드나 인자가 하나뿐인 확장 함수에 중위 호출을 사용할 수 있다.  
함수`(메소드)`를 중위 호출에 사용하게 허용하고 싶으면 infix 변경자를 함수 선언 앞에 추가해야 한다.

```kotlin
//to 함수의 정의를 간략하게 줄인 코드
infix fun Any.to(other: Any) = Pair(this, other)
```
위 to 함수는 Pair의 인스턴스를 반환한다.  
Pair의 내용으로 두 변수를 즉시 초기화 할 수 있다.

> _Pair_  
> 코틀린 표준 라이브러리 클래스로, 두 원소로 이루어진 순서쌍을 표현한다.  

```kotlin
val (number, name) = 1 to "one"
```

이런 기능을 구조 분해 선언`(destructuring declaration)`이라고 부른다.  
루프에서도 구조 분해 선언을 활용할 수 있다.  
```kotlin
//루프에서의 구조 분해 선언 활용
for ((index, element) in collection.withIndex()) {
    println("$index: $element")
}
```



---

<br>

*참고 서적 : Kotlin IN Action*