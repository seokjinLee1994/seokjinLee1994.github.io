---
layout: default
title: "[Kotlin] 코틀린 기초 봉인된 클래스"
date: 2021-08-11 +0800
last_modified_at: 2021-08-11 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(016) 

## <span style="color:orange">1. 봉인된 클래스: 클래스 계층 정의 시 계층 확장 제한</span>  
---  

```kotlin
// 인터페이스 구현을 통해 식 표현하기
interface Expr
class Num(val value: Int): Expr
class Sum(val left: Expr, val right: Expr): Expr
fun eval (e: Expr): Int = 
    when(e) {
        is Num -> e.value
        is Sun -> eval(e.right) + eval(e.left)

        // else 분기가 꼭 있어야한다.
        else -> 
            throw IllegalArgumentException("Unknown expression")
    }
```
코틀린 컴파일러는 when을 사용해 Expr 타입의 값을 검사할 때 꼭 디폴트 분기인 else 분기를 덧붙이게 강제한다. 이 예제의 else 분기에서는 반환할 만한 의미 있는 값이 없으므로 예외를 던진다.  
디폴트 분기가 있으면 이런 클래스 계층에 새로운 하위 클래스를 추가하더라도 컴파일러가 when이 모든 경우를 처리하는지 제대로 검사할 수 없다. 실수로 새로운 클래스 처리를 잊었더라고 디폴트 분기가 선택되어 심각한 버그가 발생할 수 있다.  
sealed 클래스가 그에 대한 해법이다. 상위 클래스에 sealed 변경자를 붙이면 그 상위 클래스를 상속한 하위 클래스 정의를 제한할 수 있다. sealed 클래스의 하위 클래스를 정의할 때는 반드시 상위 클래스 안에 중첩 시켜야 한다.

```kotlin
// sealed 클래스로 식 표현하기
sealed class Expr {

    // 기반 클래스의 모든 하위 클래스를 중첩 클래스로 나열한다.
    class Num(val value: Int): Expr()
    class Sum(val left: Expr, val right: Expr): Expr()
}
fun eval(e: Expr): Int = 
    when(e) {
        is Expr.Num -> e.value
        is Expr.Sum -> eval(e.right) + eval(e.left)
        //when 식이 모든 하위 클래스를 검사하므로 별도의 else 분기가 없도 된다.
    }
```
when 식에서 sealed 클래스의 모든 하위 클래스를 처리한다면 디폴트 분기가 필요 없다. sealed로 표시된 클래스는 자동으로 open이다. 따라서 별도의 open 변경자를 붙일 필요가 없다.  
<br>
sealed 클래스에 속한 값에 대해 디폴트 분기를 사용하지 않고 when 식을 사용하면 나중에 sealed 클래스의 상속 계층에 새로운 하위 클래스를 추가해도 when식이 컴파일 되지 않는다.  
sealed 인터페이스를 정의할 수는 없다. 봉인된 인터페이스를 만들 수 있다면 그 인터페이스를 자바 쪽에서 구현하지 못하게 막을 수 있는 수단이 코틀린 컴파일러에게 없기 때문이다.

---

<br>

*참고 서적 : Kotlin IN Action*