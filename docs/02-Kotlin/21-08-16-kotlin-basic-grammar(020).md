---
layout: default
title: "[Kotlin] 코틀린 기초 게터 세터 / 접근자 가시성 변경"
date: 2021-08-16 +0800
last_modified_at: 2021-08-16 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(020) 

## <span style="color:orange">1. 게터와 세터에서 뒷바침하는 필드에 접근</span>  
---  

```kotlin
// 세터에서 뒷바침하는 필드 접근하기
class User(val name: String) {
    var address: String = "unspecified"
        set(value: String) {
            println("""
                Address was changed for $name: 
                "$field"->"$value".""".trimIndent()) // 뒷바침하는 필드 값 읽기
            field = value // 뒷바침하는 필드 변경
        }
}
val user = User("Alice")
user.address = "Elsenheimerstrasse 47, 80687 Muenchen"

// 결과
// Address was changed for Alice: "unspecified" -> "Elsenheimerstrasse 47, 80687 Muenchen"
```

코틀린에서 프로퍼티의 값을 바꿀 때는 user.address= = "new value" 처럼 필드 설정 구문을 사용한다. 이 구문은 내부적으로 address의 세터를 호출한다.  
접근자의 본문에서는 field라는 특별한 식별자를 통해 뒷바침하는 필드에 접근할 수 있다. 게터는 읽기만, 세터는 읽기와 쓰기가 가능하다.  

<br><br>

## <span style="color:orange">2. 접근자의 가시성 변경</span>
---  

```kotlin
// 비공개 세터가 있는 프로퍼티 선언하기
class LengthCounter {
    var counter: Int = 0
        // 이 클래스 밖에서 이 프로퍼티의 값을 바꿀 수 없다.
        private set 
    fun addWord(word: String) {
        counter += word.length
    }
}

val lengthCounter = LengthCounter()
lengthCounter.addWord("Hi!")
println(lengthCounter.counter)
// 결과 = 3
```
전체 길이를 저장하는 프로퍼티는 클라이언트에게 제공하는 API의 일부분이므로 public으로 외부에 공개된다.
하지만 외부 코드에서 단어 길이의 합을 마음대로 바꾸지 못하게 이 클래스 내부에서만 길이를 변경하게 만들고 싶다. 그래서 기본 가시성을 가진 게터를 컴파일러가 생성하게 내버려 두는 대신 세터의 가시성을 private으로 지정한다.

---

<br>

*참고 서적 : Kotlin IN Action*