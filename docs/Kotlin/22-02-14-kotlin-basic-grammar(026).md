---
layout: default
title: "[Kotlin] 동반 객체 : 팩토리 메소드와 정적 멤버가 들어갈 장소"
date: 2022-02-14 +0800
last_modified_at: 2022-02-14 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(026) 

## <span style="color:orange">1. 동반 객체 : 팩토리 메소드와 정적 멤버가 들어갈 장소</span>  
---

코틀린 클래스 안에는 정적인 멤버가 없다.(코틀린은 자바 static 키워드 지원 X)<br>
코틀린에서는 패키지 수준의 최상위 함수<sup>[[1]](#footnote_1)</sup>와 객체 선언<sup>[[2]](#footnote_2)</sup>을 활용한다.<br>
대부분의 경우 최상위 함수를 활용하는 편을 더 권장.<br>
하지만 private으로 표시된 클래스 비공개 멤버에 접근할 수 없다. 그래서 클래스의 인스턴스와 관계없이 호출해야 하지만, 클래스 내부 정보에 접근해야 하는 함수가 필요할 때는 클래스에 중첩된 객체 선언의 멤버 함수로 정의해야 한다.(팩토리 메소드가 대표적인 예)<br>
<br>

클래스 안에 정의된 객체 중 하나에 companion이라는 특별한 표시를 붙이면 그 클래스의 동반 객체로 만들 수 있다.<sup>[[3]](#footnote_3)</sup> <sup>[[4]](#footnote_4)</sup><br>

```kotlin
class A {
    companion object {
        fun bar() {
            println("Companion object called")
        }
    }
}

A.bar()
```

```kotlin
// 부 생성자가 여럿 있는 클래스 정의하기
class User {
    val nickName : String

    constructor(email : String) {
        nickName = email.substringBefore('@')
    }
    constructor(accountId : Int) {
        nickName = getAccountName(accountId)
    }
}
```

위의 로직을 표현하는 더 유용한 방법으로 클래스의 인스턴스를 생성하는 팩토리 메소드가 있다.<sup>[[5]](#footnote_5)</sup>
```kotlin
// 부 생성자를 팩토리 메소드로 대신하기
class User private constructor(val nickname : String) {
    companion object {
        fun newSubscribingUser(email : String) = User(email.substringBefore('@'))
        fun newUser(accountId : Int) = User(getAccountName(accountId))
    }
}

// 클래스 이름을 사용해 그 클래스에 속한 동반 객체의 메소드를 호출할 수 있다.

val subscribingUser = User.newSubscribingUser("bob@gmail.com")
val accountUser = User.newUser(4)
println(subscribingUser.nickName)

>> bob
```
팩토리 메소드는 그 팩토리 메소드가 선언된 클래스의 하위 클래스 객체를 반환할 수도 있다.<sup>[[6]](#footnote_6)</sup><br>
팩토리 메소드는 생성할 필요가 없는 객체를 생성하지 않을 수도 있다.<sup>[[7]](#footnote_7)</sup><br>

---

<a name="footnote_1">1</a> : 자바의 정적 메소드 역할을 거의 대신 할 수 있다.
<br>
<a name="footnote_2">2</a> : 자바의 정적 메소드 역할 중 코틀린 최상위 함수가 대신 할 수 없는 역할이나 정적 필드를 대신할 수 있다.
<br>
<a name="footnote_3">3</a> : 동반 객체의 프로퍼티나 메소드에 접근하려면 그 동반 객체가 정의된 클래스 이름을 사용한다. 이때 객체의 이름을 따로 지정할 필요가 없다.
<br>
<a name="footnote_4">4</a> : 동반 객체는 private 생성자를 호출하기 좋은 위치다. 동반 객체는 자신을 둘러싼 클래스의 모든 private 멤버에 접근이 가능하다. 따라서 동반 객체는 바깥쪽 클래스의 private 생성자도 호출이 가능하다.(팩토리 패턴을 구현하기 가장 적합한 위치)
<br>
<a name="footnote_5">5</a> : 생성자를 통해 User 인스턴스를 만들 수 없고 팩토리 메소드를 통해야만 한다.
<br>
<a name="footnote_6">6</a> : SubscribingUser와 AccountUser 클래스가 따로 존재한다면 필요에 따라 적당한 클래스의 객체를 반환할 수 있다.
<br>
<a name="footnote_7">7</a> : 이메일 주소별로 유일한 User 인스턴스를 만드는 경우 팩토리 메소드가 이미 존재하는 인스턴스에 해당하는 이메일 주소를 전달받으면 새 인스턴스를 만들지 않고 캐시에 있는 기존 인스턴스틑 반환할 수 있다. 하지만 클래스를 확장해야만 하는 경우에는 동반 객체 멤버를 하위 클래스에서 오버라이드할 수 없으므로 여러 생성자를 사용하는 편이 더 나은 방법이다.

<br>
*참고 서적 : Kotlin IN Action*