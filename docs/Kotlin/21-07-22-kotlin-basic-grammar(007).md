---
layout: post
title: "[Kotlin] 코틀린 기초 컬렉션"
date: 2021-07-22 +0800
last_modified_at: 2021-07-22 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(007) 

## <span style="color:orange">1. 컬렉션</span>  
---  

```kotlin
//컬렉션 만들기

//setOf 함수를 이용한 집합
val set = hashSetOf(1, 7, 53)

//리스트와 맵
val list = arrayListOf(1, 7, 53)
val map = hashMapOf(1 to "one", 7 to "seven", 53 to "fifty-three")
```

> _to_  
> key to value

코틀린 컬렉션은 자바 컬렉션과 똑같은 클래스다. 하지만 코틀린에서는 자바보다 더 많은 기능을 쓸 수 있다.  
ex) 리스트의 마지막 원소를 가져오거나 수로 이뤄진 컬렉션에서 최댓값을 찾을 수 있다.
```kotlin
//마지막 원소 가져오기
val strings = listOf("first", "second", "fourtheenth")
println(strings.last())

//최댓값 찾기
val numbers = setOf(1, 14, 2)
println(numbers.max())
```  
---

<br>

*참고 서적 : Kotlin IN Action*