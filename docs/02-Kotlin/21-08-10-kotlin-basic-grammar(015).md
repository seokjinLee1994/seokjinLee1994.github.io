---
layout: default
title: "[Kotlin] 코틀린 기초 내부 클래스와 중첩된 클래스 "
date: 2021-08-10 +0800
last_modified_at: 2021-08-10 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(015) 

## <span style="color:orange">1. 내부 클래스와 중첩된 클래스: 기본적으로 중첩 클래스</span>  
---  

클래스 안에 클래스를 선언하면 도우미 클래스를 캡슐화하거나 코드 정의를 그 코드를 사용하는 곳 가까이에 두고 싶을 때 유용하다. 자바와의 차이는 코틀린의 중첩 클래스는 명시적으로 요청하지 않는 한 바깥쪽 클래스 인스턴스에 대한 접근 권한이 없다는 점이다.

```kotlin
// 직렬화 할 수 있는 상태가 있는 뷰 선언
interface State: Serializable
interface View {
    fun getCurrentState(): State
    fun restoreState(state:State) {}
}
```

```java
// 자바에서 내부 클래스를 사용해 View 구현하기
public class Button implements View {
    @Override
    public State getCurrentState() {
        return new ButtonState();
    }
    @Override
    public void restoreState(State state) { /*...*/ }
    
    public class ButtonState implements State { /*...*/ }
}
```

해당 코드에서 선언한 버튼의 상태를 직렬화하면 java.io.NotSerializableException: Button이라는 오류가 발생한다.  
자바에서 다른 클래스 안에 정의한 클래스는 자동으로 내부 클래스가 된다. 이 예제의 ButtonState클래스는 바깥쪽 Button 클래스에 대한 참조를 묵시적으로 포함한다. 그 참조로 인해 ButtonState를 직렬화할 수 없다. Button을 직렬화할 수 없으므로 버튼에 대한 참조가 ButtonState의 직렬화를 방해한다.  
문제의 해결을 위해선 ButtonState를 static 클래스로 선언해야 한다. 그렇게 하면 해당 클래스를 둘러싼 바깥쪽 클래스에 대한 묵시적인 참조가 사라진다.

```kotlin
// 중첩 클래스를 사용해 코틀린에서 View 구현하기
class Button: View {
    override fun getCurrentState(): State = ButtonState()
    override fun restoreState(state: State) { /*...*/ }
    
    // 이 클래스는 자바의 정적 중첩 클래스와 대응한다.
    class ButtonState: state { /*...*/ }
}
```

코틀린 중첩 클래스에 아무런 변경자가 붙지 않으면 자바 static 중첩 클래스와 같다.  
내부 클래스로 변경해서 바깥쪽 클래스에 대한 참조를 포함하게 만들고 싶다면 inner 변경자를 붙여야한다.

`자바와 코틀린의 중첩 클래스와 내부 클래스의 관계`
클래스 B안에 정의된 클래스 A | 자바 | 코틀린
---|---|---|
중첩 클래스(바깥쪽 클래스에 대한 참조를 저정하지 않음) | static class A | class A
내부 클래스(바깥쪽 클래스에 대한 참조를 저장함) | class A | inner class A

코틀린에서 바깥쪽 클래스의 인스턴스를 가리키는 참조를 표기하는 방법도 자바와 다르다. 내부 클래스 Inner 안에서 바깥쪽 클래스 Outerdml 참조에 접근하려면 this@Outer라고 써야 한다.
```kotlin
class Outer {
    inner class Inner {
        fun getOuterReference(): Outer = this@Outer
    }
}
```

---

<br>

*참고 서적 : Kotlin IN Action*