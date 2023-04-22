---
layout: default
title: "[Kotlin] 코틀린 기초 클래스 초기화"
date: 2021-08-12 +0800
last_modified_at: 2021-08-12 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(017) 

## <span style="color:orange">1. 클래스 초기화: 주 생성자와 초기화 블록</span>  
---  

```kotlin
// 간단한 클래스 선언 방법
class User(val nickname: String)
```
보통 클래스의 모든 선언은 {} 사이에 들어간다. 하지만 이 클래스 선언에는 {}가 없고 () 사이에 val 만 존재한다. 이렇게 클래스 뒤에 오는 ()로 둘러싸인 코드를 주 생성자라고 부른다.  
주 생성자는 생성자 파라미터를 지정하고 그 생성자 파라미터에 의해 초기화되는 프로퍼티를 정의하는 두 가지 목적에 쓰인다.

```kotlin
// 파라미터가 하나만 있는 주 생성자
class User constructor(_nickname: String) {
    val nickname: String

    // 초기화 블록
    init {
        nickname+ _nickname
    }
}
```
constructor 키워드는 주 생성자나 부 생성자 정의를 시작할 때 사용한다. init 키워드는 초기화 블록을 시작한다. 초기화 블록에는 클래스의 객체가 만들어질 때 실행될 초기화 코드가 들어간다. 주 생성자는 제한적이기 때문에 별도의 코드를 포함할 수 없으므로 초기화 블록이 필요하다. 필요하다면 클래스 안에 여러 초기화 블록을 선언할 수 있다.

```kotlin
class User(_nickname: String) {
    val nickname = _nickname
}
```
nickname 프로퍼티를 초기화하는 코드를 nickname 프로퍼티 선언에 포함시킬 수 있어서 초기화 코드를 초기화 블록에 넣을 필요가 없다.  
주 생성자 앞에 별다른 애노테이션이나 가시성 변경자가 없다면 constructor를 생략해도 된다.

```kotlin
// 생성자 파라미터에 디폴트 값 정의
class user(val nickname: String, val isSubscribed: Boolean = false)
```

```kotlin
open class User(val nickname: String) { ... }
class TwitterUser(nickname: String) : User(nickname) { ... }
```
클래스에 기반 클래스가 있다면 주 생성자에서 기반 클래스의 생성자를 호출해야 할 필요가 있다. 기반 클래스를 초기화하려면 기반 클래스 이름 뒤에 괄호를 치고 생성자 인자를 넘긴다.



---

<br>

*참고 서적 : Kotlin IN Action*