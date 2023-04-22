---
layout: default
title: "[Kotlin] 코틀린 기초 확장함수와 확장 프로퍼티"
date: 2021-07-26 +0800
last_modified_at: 2021-07-26 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(008) 

## <span style="color:orange">1. 확장 함수</span>  
---  
확장 함수를 만들려면 추가하려는 함수 이름 앞에 그 함수가 확장할 클래스의 이름을 덧붙이기만 하면 된다,  
클래스 이름을 수신 객체 타입`(receiver type)`이라 부르며, 확장 함수가 호출되는 대상이 되는 값`(객체)`을 수신 객체`(receiver object)`라고 부른다.
```kotlin
fun String.lastChar(): Char = this.get(this.length - 1)

println("Kotlin".lastChar())

//String = 수신 객체 타입
//this = 수식 객체
//수신 객체를 생략할 수도 있다.
```

임포트와 확장 함수  
```kotlin
//개별 함수 임포트
import strings.lastChar

//*을 사용한 전체 함수 임포트
import strings.*

//as 키워드를 사용해 함수를 다른 이름으로 부르기
import strings.lastChar as last
```

<br><br>

## <span style="color:orange">2. 확장 프로퍼티</span>
---
확장 프로퍼티를 사용하면 기존 클래스 객체에 대한 프로퍼티 형식의 구문으로 사용할 수 있는 API를 추가할 수 있다. 프로퍼티라는 이름으로 불리기는 하지만 상태를 저장할 적절한 방법이 없기 때문에 실제로 프로퍼티는 아무 상태도 가질 수 없다.  
하지만 프로퍼티 문법으로 더 짧게 코드를 작성할 수 있어 편한 경우가 있다.

```kotlin
//확장 프로퍼티 선언
val String.lastChar: Char
    get() = get(length - 1)
```
뒷바침하는 필드가 없어서 기본 게터 구현을 제공할 수 없으므로 푀소한 게터는 꼭 정의를 해야한다.  
초기화 코드에서 계산한 값을 담을 장소가 전혀 없으므로 초기화 코드도 쓸 수 없다.

```kotlin
//변경 가능한 확장 프로퍼티 선언
var StringBuilder.lastChar: Char
    get() = get(length - 1)
    set(value: Char) {
        this.setCharAt(length - 1, value)
    }

val sb = StringBuilder("Kotlin?")
sb.lastChar = '!'
println(sb)
//결과 값: Kotlin!
```

---

<br>

*참고 서적 : Kotlin IN Action*