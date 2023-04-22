---
layout: default
title: "[Kotlin] 코틀린 객체 선언"
date: 2021-08-23 +0800
last_modified_at: 2022-02-14 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(025) 

## <span style="color:orange">1. 객체 선언: 싱글턴을 쉽게 만들기</span>  
---  

자바에서는 보통 클래스의 생성자를 private으로 제한하고 정적인 필드에 그 클래스의 유일한 객체를 저장하는 싱글턴 패턴을 통해 구현한다.  
코틀린은 객체 선언 기능을 통해 싱글턴을 언어에서 기본 지원한다. 객체 선언은 클래스 선언과 그 클래스에 속한 단일 인스턴스의 선언을 합친 선언이다.

```kotlin
// 객페 선언의 예시
object Payroll {
    val allEmployees = arrayListOf<Person>()
    fun calculateSalary() {
        for (person in allEmployees) {
            ...
        }
    }
}

// 객체 선언으로 만들어진 싱글턴 객체의 메소드, 프로퍼티에 접근
Payroll.allEmployees.add(Person(...))
Payroll.calculateSalary()
```

- 객체 선언은 object 키워드로 시작한다.  
- 클래스를 정의하고 그 클래스의 인스턴스를 만들어서 변수에 저장하는 모든 작업을 단 한 문장으로 처리한다.  
- 객체 선언 안에도 프로퍼티, 메소드, 초기화 블록 등이 들어갈 수 있다.  
- 생성자는 객체 선언에 쓸 수 없다. 싱글턴 객체는 객체 선언문이 있는 위치에서 생성자 호출 없이 즉시 만들어진다.  
- 변수와 마찬가지로 객체 선언에 사용한 이름 뒤에 .을 붙이면 객체에 속한 메소드, 프로퍼티에 접근이 가능하다.

객체 선언도 글래스나 인터페이스를 상속할 수 있다. 프레임워크를 사용하기 위해 특정 인터페이스를 구현해야 하는데, 그 구현 내부에 다른 상태가 필요하지 않은 경우에 이런 기능이 유용하다.  

```kotlin
// 객체 선언을 사용해 Comparator 구현하기
object CaseInsensitiveFileComparator: Comparator<File> {
    override fun compare(file: File, file2: File): Int {
        return file1.path.compareTo(file2.path, ignoreCase = true)
    }
}
```

일반 객체(클래스 인스턴스)를 사용할 수 있는 곳에서는 항상 싱글턴 객체를 사용할 수 있다. 예를 들어 이 객체를 Comparator를 인자로 받는 함수에게 인자로 넘길 수 있다.
```kotlin
// 전달받은 Comparator에 따라 리스트를 정렬하는 sortedWith 함수를 사용
val files = listOf(File("/Z"), File("/a"))
println(files.sortedWith(CaseInsensitiveFileComparator))
// 결과 = [/a, /Z]
```

> _싱글턴과 의존관계 주입_  
> 싱글턴 패턴과 마찬가지 이유로 대규모 소프트웨어 시스템에서는 객체 선언이 항상 적합하지는 않다. 
> 의존관계가 별로 많지 않은 소규모 소프트웨어에서는 싱글턴이나 객체 선언이 유용하지만, 시스템을 구현하는 다양한 구성 요소와 상호작용하는 대규모 컴포넌트에는 싱글턴이 적합하지 않다. 이유는 객체 생성을 제어할 방법이 없고 생성자 파라미터를 지정할 수 없어서다.  
>  
> 생성을 제어할 수 없고 생성자 파라미터를 지정할 수 없으므로 단위 테스트를 하거나 소프트웨어 시스템의 설정이 달라질 때 객체를 대체하거나 객체의 의존관계를 바꿀 수 없다. 따라서 그런 기능이 필요하다면 자바와 마찬가지로 의존관계 주입 프레임워크와 코틀린 클래스를 함께 사용해야 한다.  

```kotlin
// 중첩 객체를 사용해 Comparator 구현하기
data class Person(val name: String) {
    object NameComparator: Comparator<Person> {
        override fun compare (p1: Person, p2: Person) : Int = 
            p1.name.compareTo(p2.name)
    }
}

val persons = listOf(Person("Bob"), Person("Alice"))
println(persons.sortedWith(Person.NameComparator))
// 결과 = [Person(name=Alice), Person(name=Bob)]
```
<br><br>

## <span style="color:orange">2. 중첩 객체를 사용하여 Comparator 구현하기</span>  
---

```kotlin
data class Person(val name : String) {
    object NameComparator : Comparator<Person> {
        override fun compare(p1 : Person, p2 : Person) : Int = 
            p1.name.compareTo(p2.name)
    }
}
val persons = listOf(Person("Bob"), Person("Alice"))
println(persons.sortWith(Person.NameComparator))

>> [Person(name=Alice), Person(name-Bob)]
```

---

<br>

*참고 서적 : Kotlin IN Action*