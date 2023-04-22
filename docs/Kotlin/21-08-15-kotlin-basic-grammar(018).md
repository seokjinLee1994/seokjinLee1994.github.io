---
layout: post
title: "[Kotlin] 코틀린 기초 부 생성자"
date: 2021-08-15 +0800
last_modified_at: 2021-08-15 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(018) 

## <span style="color:orange">1. 부 생성자: 상위 클래스를 다른 방식으로 초기화</span>  
---  

```kotlin
open class View {
    constructor(context: Context) { /*...*/ }
    constructor(context: Context, attr: AttributeSet) { /*...*/ }
}
```
이 클래스는 주 생성자를 선언하지 않고(클래스 헤더에 있는 클래스 이름 뒤에 괄호가 없다.), 부 생성자만 2가지 선언한다. 부 생성자는 constructor 키워드로 시작한다. 필요에 따라 얼마든지 부 생성자를 많이 선언해도 된다.

```kotlin
class MyButton: View {
    constructor(context: Context): super(context) { /*...*/ }
    constructor(context: Context, attr: AttributeSet): super(context, attr) { /*...*/ }
}
```
두 부 생성자는 super() 키워드를 통해 자신에 대응하는 상위 클래스 생성자를 호출한다.

```kotlin
class MyButton: View {
    
    // 이 클래스의 다른 생성자에게 위임한다.
    constructor(context: Context): this(context, MY_STYLE) { /*...*/ }

    constructor(context: Context, attr: AttributeSet): super(context, attr) { /*...*/ }
}
```
MyButton 클래스의 생성자 중 하나가 파라미터의 디폴트 값을 넘겨서 같은 클래스의 다른 생성자(this를 사용해 참조함)에게 생성을 위임한다. 두 번째 생성자는 여전히 super()를 호출한다.  
첫 번째 생성자는 두 번째 생성자에게 생성을 위임하고 두 번째 생성자는 상위 클래스에 생성을 위임한다.  
부 생성자가 필요한 주된 이유는 자바 상호운용성이다. 하지만 부 생성자가 필요한 다른 경우도 있다. 클래스 인스턴스를 생성할 때 파라미터 목록이 다른 생성 방법이 여럿 존재하는 경우에는 부 생성자를 여럿 둘 수밖에 없다.

---

<br>

*참고 서적 : Kotlin IN Action*