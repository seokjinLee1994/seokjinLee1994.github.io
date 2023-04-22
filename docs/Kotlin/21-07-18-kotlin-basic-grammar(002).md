---
layout: post
title: "[Kotlin] 코틀린 기초 문법 함수"
date: 2021-07-18 +0800
last_modified_at: 2021-07-18 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---
# kotlin의 기본 문법 정리(002) 함수 선언

## <span style="color:orange">함수(Function)</span>
---
코틀린에서 함수는 "fun"이라는 키워드로 정의한다. 간단하게 리턴값이 있고, 없는 함수와 바디 내용이 식으로만 이루어졌을때 함수를 간략화 하는 방법은 아래와 같다.

return 값이 없는 함수 선언
```kotlin
fun welcome() {
    println("welcome to my blog")
}
```

return 값이 있는 함수 선언
```kotlin
fun max(a: Int, b: Int): Int {
    return if (a > b) a else b
}
//위의 함수를 간략화 시킨 코드
fun max (a: Int, b: Int): Int = if (a > b) a else b
//반환 타입을 생략 할 수도 있다.
fun max (a: Int, b: Int) = if (a > b) a else b
```

---

<br>

*참고 서적 : Kotlin IN Action*