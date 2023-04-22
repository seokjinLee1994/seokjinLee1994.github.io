---
layout: default
title: "[Kotlin] 코틀린 기초 예외 처리"
date: 2021-07-21 +0800
last_modified_at: 2021-07-21 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(006) 예외 처리

## <span style="color:orange">1. try, catch, finally</span>  
---  

```kotlin
//자바와 마찬가지로 try 사용하기
fun readNumber(reader: BufferedReader): Int? {
    try {
        val line = reader.readLine()
        return Integer.parseInt(line)
    } catch (e: NumberFormatException) {
        return null
    } finally {
        reader.close()
    }
}
```

코틀린은 체크 예외와 언체크 예외를 구별하지 않는다. 
> _체크 예외와 언체크 예외_  
> 언체크 예외는 RuntimeExceptio을 상속한 것들을 말한다.  
> 체크 예외는 그 외의 것들을 말한다.  

<br><br>

## <span style="color:orange">2. try를 식으로 사용</span>
---

```kotlin
//try를 식으로 사용하기
//catch에서 값 반환하기
fun readNumber(reader: BufferedReader) {
    val number = try {
        //이 식의 값이 try식의 값이 된다.
        Integer.parseInt(reader.readLine())
    } catch (e: NumberFormatException) {
        //예외가 발생하면 null 값을 사용하게 된다.
        null
    }
    println(number)
}
```

<br>

---

<br>

*참고 서적 : Kotlin IN Action*