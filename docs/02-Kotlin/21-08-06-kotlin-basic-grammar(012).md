---
layout: default
title: "[Kotlin] 코틀린 기초 코틀린 인터페이스"
date: 2021-08-06 +0800
last_modified_at: 2021-08-06 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(012) 

## <span style="color:orange">1. 코틀린 인터페이스</span>  
---  
코틀린 인터페이스는 자바 8 인터페이스와 비슷하다. 코틀린 인터페이스 안에는 추상 메소드뿐 아니라 구현이 있는 메소드도 정의할 수 있다. 다만 인터페이스에는 아무런 필드도 들어갈 수 없다.

```kotlin
// 간단한 인터페이스 선언하기
interface Clickable {
    fun click()
}
```

```kotlin
// 단순한 인터페이스 구현하기
class Button: Clickable {
    override fun click() = println("I was clicked")
}
Button().click()
```
자바에는 extends와 implements 키워드를 사용하지만, 코틀린에서는 클래스 이름 뒤에 콜론(:)을 붙이고 인터페이스와 클래스 이름을 적는 것으로 클래스 확장과 인터페이스 구현을 모두 처리한다.  
자바와 마찬가지로 클래스는 인터페이스를 원하는 만큼 개수 제한 없이 마음대로 구현할 수 있지만, 클래스는 오직 하나만 확장할 수 있다.

```kotlin
// 인터페이스 안에 본문이 있는 메소드 정의하기
interface Clickable {
    // 일반 메소드 선언
    fun click()
    // 디폴트 구현이 있는 메소드
    fun showOff() = ptintln("I'm clickable!")
}
```

이 인터페이스를 구현하는 클래스는 click에 대한 구현을 제공해야 한다. 반면 showOff 메소드의 경우 새로운 동작을 정의할 수도 있고, 그냥 정의를 생략해서 디폴트 구현을 사용할 수도 있다.

```kotlin
// 동일한 메소드를 구현하는 다른 인터페이스 정의
interface Focusable {
    fun setFocus(b: Boolean) = println("I ${if (b) "got" else "loast"} focus.")
    fun showOff() = println("I'm focusable!")
}
```

showOff라는 디폴트 구현이 있는 Clickable과 Focusable 두 인터페이스를 한 클래스에서 함께 구현하면 어느쪽 showOff 메소드도 선택되지 않는다. 클래스가 구현하는 두 상위 인터페이스에 정의된 showOff 구현을 대체할 오버라이딩 메소드를 직접 제공하지 않으면 다음과 같은 컴파일러 오류를 보게된다.

`The class 'Button' must  
override public open fun showOff() because it inherits  
many implementations of it.`

코틀린 컴파일러는 두 메소드를 아우르는 구현을 하위 클래스에 직접 구현하게 강제한다.

```kotlin
// 상속한 인터페이스의 메소드 구현 호출하기
class Button: Clickable, Focusable {
    override fun click() = println("I was clicked")

    // 이름과 시그니처가 같은 멤버 메소드에 대해 둘 이상의 디폴트 구현이 있는 경우 
    // 인터페이스를 구현하는 하위 클래스에서 명시적으로 새로운 구현을 제공해야 한다.
    override fun showOff() {

        // 상위 타입의 이름을 <> 사이에 넣어 super를 지정하면
        // 어떤 상위 타입의 멤버 메소드를 호출할지 지정할 수 있다.
        super<Clickable>.showOff()
        super<Focusable>.showOff()
    }
}
```

Button은 상속한 두 상위 타입의 showOff() 메소드를 호출하는 방식으로 showOff()를 구현한다.  
자바에서는 Clickable.super.showOff() 처럼 super 앞에 기반 타입을 적지만,  
코틀린에서는 super&lt;Clickable&gt;.showOff()처럼 <>안에 기반 타입 이름을 지정한다.

```kotlin
// 상속한 구현 중 단 하나만 호출할 경우 다음과 같이 쓸 수 있다.
override fun showOff() = super<Clickable>.showOff()
```
<br><br>

---

<br>

*참고 서적 : Kotlin IN Action*