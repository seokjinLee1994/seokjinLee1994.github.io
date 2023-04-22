---
layout: default
title: "[Kotlin] 코틀린 기초 문법 var / val"
date: 2021-07-16 +0800
last_modified_at: 2021-07-16 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---
# kotlin의 기본 문법 정리(001) var과 val

<br>

기존 자바에서의 변수 선언 방식  
```java
int age = 28;
```

코틀린에서의 변수 선언 방식

```kotlin
// 변수의 데이터 타입을 생략하고 선언 가능
val ageVal = 28
var ageVar = 28

// 데이터 타입을 명시하는 것도 가능
var nameVar:String = "이석진"
```

<br>

키워드 | 의미 
---|:--- 
`val` | - 변경 불가능한 참조를 저장하는 변수로서, 특정한 값을 의미하는 'Value'를 나타 냅니다.<br> - val로 선언하면 초기화 이후 '변수의 재 대입'이 불가능합니다.<br> - Java에서 'final' 키워드로 선언하는 것과 같다.
`var` | - 변경 가능한 참조, 변경 가능하다는 의미의 'Variable'을 나타낸다.<br> - Java의 일반적인 변수에 해당한다.

<br>

코틀린이 val, var 2가지 키워드를 사용하는 것은 <span style="color:orange"><u>변수의 불변성을 보장하기 위한 것</u></span>이고

타입을 따로 적지 않은 것은 <span style="color:orange"><u>타입 추론</u></span>을 하기 때문이다.
<br><br>

> _변수의 불변성?_
> - 변수의 값을 변할 수 없게 하는 것
> - 변수의 값을 변경 불가능하게 설정하면 프로그램은 해당 변수가 변경되지 않을 것임을 미리 알고 있으므로<br>시스템이 변수의 메모리를 보다 효율적으로 관리할 수 있게 된다.
> - '동시에 접근하더라도 변수의 불변성이 보장'되기 때문에 다른 쓰레드가 값을 변경할 것을 신경 쓸 필요가 없다.<br>
따라서 '멀티 쓰레드' 프로그래밍에서도 유용하다.

<br>

---

<br>

*참고 서적 : 안드로이드 Kotlin 앱 프로그래밍 가이드*
