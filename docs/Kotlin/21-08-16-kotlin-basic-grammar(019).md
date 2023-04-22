---
layout: default
title: "[Kotlin] 코틀린 기초 인터페이스에 선언된 프로퍼티 구현"
date: 2021-08-16 +0800
last_modified_at: 2021-08-16 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(019) 

## <span style="color:orange">1. 인터페이스에 선언된 프로퍼티 구현</span>  
---  

```kotlin
// 인터페이스의 프로퍼티 구현하기

//주 생성자에 있는 프로퍼티
class PrivateUser(override val nickname: String) : User

class SubscribingUser(val email: String): User {
    override val nickname: String
        get() = email.substringBefore('@') // <- 커스텀 게터
}

class FacebookUser(val accountId: Int): User {

    // 프로퍼티 초기화 식
    override val nickname = getFacebookName(accountId)
}
```

_PrivateUser_는 주 생성자 안에 프로퍼티를 직접 선언하는 간결한 구문을 사용한다. 이 프로퍼티는 User의 추상 프로퍼티를 구현하고 있으므로 override를 표시해야 한다.  
_SubscribingUSer_는 커스텀 게터로 nickname 프로퍼티를 설정한다. 이 프로퍼티는 뒷받침하는 필드에 값을 저장하지 않고 매번 이메일 주소에서 별명을 계산해 반환한다.  
_FacebookUser_에서는 초기화 식으로 nickname 값을 초기화한다. 이때 페이스북 사용자 ID를 받아서 그 사용자의 이름을 반환해주는 getFacebookName 함수(이 함수는 다른 곳에 정의돼 있다고 가정)를 호출해서 nickname을 초기화한다. getFacebookName은 페이스북에 접속해서 인증을 거친 후 원하는 데이터를 가져와야 하기 때문에 비용이 많이 들 수도 있다. 그래서 객체를 초기화하는 단계에 한 번만 getFacebookName을 호출하게 설계했다.

SubscribingUser와 FacebookUser의 nickname은 비슷해 보이지만 SubscribingUser의 nickname은 매번 호출될 때마다 substringBefore를 호출해 계산하는 커스텀 게터를 활용하고 FacebookUser의 nickname은 객체 초기화 시 계산한 데이터를 뒷바침하는 필드에 저장했다가 불러오는 방식을 활용한다.

---

<br>

*참고 서적 : Kotlin IN Action*