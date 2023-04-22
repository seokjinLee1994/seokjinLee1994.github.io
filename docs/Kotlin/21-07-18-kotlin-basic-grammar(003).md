---
layout: default
title: "[Kotlin] 코틀린 기초 문법 클래스와 프로퍼티"
date: 2021-07-18 +0800
last_modified_at: 2021-07-18 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(003) 클래스(class)와 프로퍼티(property)

## <span style="color:orange">1. 클래스(class)</span>  
---  
클래스(class)는 객체 지향 프로그래밍(OOP)에서 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀이다. 객체를 정의 하기 위한 상태(멤버변수)와 메서드(함수)로 구성된다.  
템플릿을 사용하면 객체를 클래스로 정의할 때 멤버의 자료형을 미리 정하지 않고 객체를 사용할 때 결정할 수 있다. 이를 통해 클래스나 멤버의 중복 정의를 하지 않아도 되므로 효율적으로 코딩이 가능하다.  
객체는 클래스로 규정된 인스턴스로서, 변수 대신 실제값을 가진다.  

```java
//간단한 자바 클래스 예시
public class Person {
    private final String name;
    
    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```
필드가 둘 이상으로 늘어나면 생성자인 Person(String name)의 본문에서 파라미터를 이름이 같은 필드에 대입하는 대입문의 수도 늘어난다.  
자바에서는 생성자 본문에 이 같은 코드가 반복적으로 들어가는 경우가 많다.  
코틀린에서는 그런 필드 대입 로직을 더 적은 코드로 작성할 수 있다.  
<br>

```kotlin
//코틀린에서의 표현
class Person (val name: String)
```
이런 유형의 클래스`(코드가 없이 데이터만 저장하는 클래스)`를 값 객체`(value object)`라 부른다.  
public 가시성 변경자`(visibility modifier)`가 사라졌음을 확인 할 수 있다. 코틀린의 기본 가시성은 public이므로 이런 경우 변경자를 생략해도 된다.  
<br><br>

## <span style="color:orange">2. 프로퍼티(property)</span>
---
자바에서는 필드와 접근자를 한데 묶어 프로퍼티(property)라고 부르며, 프로퍼티라는 개념을 활용하는 프레임워크가 많다.  
코틀린은 프로퍼티를 언어 기본 기능으로 제공하며, 코틀린 프로퍼티는 자바의 필드와 접근자 메소드를 완전히 대신한다.  
클래스에서 프로퍼티를 선언할 때는 val이나 var을 사용한다.  
>val = 읽기전용  
>var = 변경 가능

```kotlin
class Person {
    /**
    1. 읽기 전용 프로퍼티
    2. 코틀린은 (비공개)필드와 필드를 읽는 단순한 (공개)게터를 만들어 낸다.
    **/
    val name: String,

    /**
    1. 쓸 수 있는 프로퍼티
    2. 코틀린은 (비공개)필드, (공개)게터, (공개)세터를 만들어낸다.
    **/
    var isMarried: Boolean
}
```
기본적으로 코틀린에서 프로퍼티를 선언하는 방식은 프로퍼티와 관련있는 접근자를 선언하는 것.  
값을 저장하기 위한 비공개 필드와 그 필드에 값을 저장하기 위한 setter와 필드의 값을 읽기 위한 getter로 이루어진 간단한 디폴트 접근자 구현을 제공한다.  
<br><br>

## <span style="color:orange">3. 클래스를 사용하는 방법</span>
---
```java
//자바에서의 예시

Person person = new Person("Bob", true);
System.out.println(person.getName());
System.out.println(person.getMaeeied());
```  
```kotlin
//코틀린에서의 예시

//new 키워드를 사용하지 않고 생성자를 호출한다.
val person = Person("Bob", true)

//프로퍼티의 이름을 직접 사용해도 코틀린이 자동으로 getter를 호출해준다.
println(person.name)
println(person.isMarried)
```

> 자바와 코틀린에서의 setter 사용  
> _Java_  
> person.setMarried(false);   
>  
>  _Kotlin_  
> person.isMarried = false  
  
<br><br>

## <span style="color:orange">4. 커스텀 접근자</span>
---
```kotlin
//정사각형인지 직사각형인지를 알려주는 기능을 가진 Rectangle클래스 정의
class Rectangle(val height: Int, val width: Int) {
    val isSquare: Boolean

    get() {
        return height == width
    }

    //블록을 본문으로 하는 구문을 꼭 사용할 필요는 없다. 아래와 같이 커스텀 접근자(get())를 만들 수 있다.
    //get() = height == width
}

val rectangle = Rectangle(41, 43)
println(rectangle.isSquare)

```

> _파라미터가 없는 함수 정의 방식 vs 커스텀 게터를 정의하는 방식_  
> 두 방식 모두 비슷하다.  
> 구현이나 성능상 차이는 없다. 차이가 나는 부분은 가독성 뿐이다.

<br>

---

<br>

*참고 서적 : Kotlin IN Action*