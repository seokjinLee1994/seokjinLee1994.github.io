---
layout: default
title: "[Kotlin] 코틀린 기초 코드 다듬기: 로컬 함수와 확장"
date: 2021-07-30 +0800
last_modified_at: 2021-07-30 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(011) 

코틀린에서는 함수에서 추출한 함수를 원 함수 내부에 중첩시킬 수 있다. 그렣게 하면 문법적인 부가 비용을 들이지 않고도 깔끔하게 코드를 조직할 수 있다.

## <span style="color:orange">1. 로컬 함수와 확장</span>  
---  

```kotlin
//코드 중복을 보여주는 예제
class User (val id: Int, val name: String, val address: String)

fun saveUser (user: User) {
    if (user.name.isEmpty()) {
        throw IllegalArgumentException ("Can't save user $(user.id): empty Name")
    }
    if (user.address.isEmpty()) {
        throw IllegalArgumentException ("Can't save user $(user.id): empty Address")
    }
    //user를 데이터베이스에 저장한다.
}
saveUser(User(1, "", ""))
//결과 = java.lang.IllegalArgumentException: Can't save user 1: empty Name
```

클래스가 사용자의 필드를 검증할 때 필요한 여러 경우를 하나씩 처리하는 메소드로 넘쳐나기를 바라지는 않을 것이다. 이런 경우 검증 코드를 로컬 함수로 분리하면 중복을 없애는 동시에 코드 구조를 깔끔하게 유지할 수 있다.

```kotlin
//로컬 함수를 사용해 코드 중복 줄이기
class User (val id: Int, val name: String, val address: String) 

fun saveUser (user: User) {
    fun validate (user: User, value: String, fieldName: Stiring) {
        if (value.isEmpty()) {
            throw IllegalArgumentException ("Can't save user $(user.id): empty $fieldName")
        }
    }
    validate (user, user.name, "Name")
    validate (user, user.address, "Address")
    //user를 데이터베이스에 저장한다.
}
```

검증 로직 중복은 사라졌고, 필요하면 User의 다른 필드에 대한 검증도 쉽게 추가할 수 있다.  
로컬 함수는 자신이 속한 바깥 함수의 모든 파라미터와 변수를 사용할 수 있다. 이런 성질을 이용해 불필요한 User 파라미터를 없앨 수 있다.

```kotlin
//로컬 함수에서 바깥 함수의 파라미터 접근하기
class User (val id: Int, val name: String, val address: String) 

fun saveUser (user: User) {
    //user 파라미터를 받을 필요 없이 saveUser()의 user를 사용할수 있다.
    fun validate (value: String, fieldName: Stiring) {
        if (value.isEmpty()) {
            //바깥 함수의 파라미터에 직접 접근할 수 있다.
            throw IllegalArgumentException ("Can't save user $(user.id): empty $fieldName")
        }
    }
    validate (user, user.name, "Name")
    validate (user, user.address, "Address")
    //user를 데이터베이스에 저장한다.
}
```

이 코드를 더 개선하고 싶다면 검증 로직을 User 클래스를 확장한 함수로 만들 수도 있다.

```kotlin
class User (val id: Int, val name: String, val address: String)

fun User.validateBeforeSave() {
    fun validate(value: String, fieldName: String) {
        if (value.isEmpty()) {
            //User의 프로퍼티를 직접 사용 가능
            throw IllegalArgumentException("Can't save user $id: empty $fieldName")
        }
    }
    validate (name, "Name")
    validate (address, "Address")
}
fun saveUser (user: User) {
    user.validateBeforeSave()
    //user를 데이터베이스에 저장한다.
}
```

확장 함수를 로컬 함수로 정의할 수도 있다. 하지만 중첩된 함수의 깊이가 깊어지면 코드를 읽기가 상당히 어려워진다. 따라서 일반적으로는 한 단계만 함수를 중첩시키라고 권장한다.  

<br><br>

---

<br>

*참고 서적 : Kotlin IN Action*