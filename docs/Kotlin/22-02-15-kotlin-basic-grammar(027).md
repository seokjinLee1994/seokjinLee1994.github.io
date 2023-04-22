---
layout: default
title: "[Kotlin] 동반 객체를 일반 객체처럼 사용"
date: 2022-02-15 +0800
last_modified_at: 2022-02-15 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(027) 

## <span style="color:orange">동반 객체를 일반 객체처럼 사용</span>  
---

동반 객체는 클래스 안에 정의된 일반 객체다. 따라서 동반 객체에 이름을 붙이거나, 동반 객체가 인터페이스를 상속하거나, 동반 객체 안에 확장 함수와 프로퍼티를 정의할 수 있다.<br>

```kotlin
class Person(val name: String) {
    // 동반 객체에 이름을 붙일 수 있다.
    companion object Loader{
        fun fromJSON(jsonText: String) : Preson = ...
    }
}

// 두 방법 모두 fromJSON을 호출할 수 있다.
Person.Loader.fromJSON("{name: 'Bob'}")
Person.fromJSON("{name: 'Bob'}")
```
<br>
<br>

*<h2>동반 객체에서 인터페이스 구현</h2>*

---
```kotlin
// 동반 객체에서 인터페이스 구현하기
interface JSONFactory<T> {
    fun fromJSON(jsonText: String): T
}

class Person(val name : String) {
    companion object : JSONFactory<Person> {
        override fun fromJSON(jsonText : String) : Person...
    }
}
```

> _코틀린 동반 객체와 정적 멤버_<br>
> 클래스의 동반 객체는 일반 객체와 비슷한 방식으로, 클래스에 정의된 인스턴스를 가리키는 정적 필드로 컴파일된다.<br>
> 동반 객체에 이름을 붙이지 않았다면 자바 쪽에서 Companion이라는 이름으로 그 참조에 접근할 수 있다.<br>

<br>
<br>

*<h2>동반 객체 확장</h2>*

---
클래스에 동반 객체가 있으면 그 객체 안에 함수를 정의함으로써 클래스에 대해 호출할 수 있는 확장 함수를 만들 수 있다.<sup>[[1]](#footnote_1)</sup><br>

```kotlin
// 동반 객체에 대한 확장 함수 정의

// 비즈니스 로직 모듈
class Person(val firsName: String, val lastName: String) {
    // 비어있는 동반 객체를 선언
    companion object {

    }
}

// 클라이언트 / 서버 통신 모듈
// 확장함수를 선언
fun Person.Companion.fromJSON(json: String): Person {
    ...
}

val person = Person.fromJSON(json)
```

동반 객체 안에서 fromJSON 함수를 정의한 것처럼 호출할 수 있다.<sup>[[2]](#footnote_2)</sup><sup>[[3]](#footnote_3)</sup><br>

---

<a name="footnote_1">1</a> : 예를들어 Person이라는 클래스 안에 동반 객체가 있고 그 동반 객체 안에 getAge()라는 함수를 정의하면 외부에서는 Person.getAge()로 호출할 수 있다.<br>
<a name="footnote_2">2</a> : 실제로는 fromJSON 함수는 클래스 밖에서 정의한 확장 함수다.<br>
<a name="footnote_3">3</a> : 동반 객체에 대한 확장 함수를 작성할 수 있으려면 원래 클래스에 동반 객체를 꼭 선언해야한다.<br>
<br>
*참고 서적 : Kotlin IN Action*