---
layout: post
title: "[Kotlin] 코틀린 기초 문자열과 정규식 다루기"
date: 2021-07-29 +0800
last_modified_at: 2021-07-29 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(010) 

코틀린 문자열은 자바 문자열과 같다. 코틀린 코드가 만들어낸 문자열을 아무 자바 메소드에 넘겨도 되며, 자바 코드에서 받은 문자열을 아무 코틀린 표준 라이브러이 함수에 전달해도 전혀 문제없다.  

## <span style="color:orange">1. 문자열 나누기</span>  
---  
자바 split 메소드로는 점(.)을 사용해 문자열을 분리할 수 없다.  
split의 구분 문자열은 정규식이기 때문이다.  
따라서 마침표는 모든 문자를 나타내는 정규식으로 해석된다.  
코틀린에서는 자바의 split 대신에 여러가지 다른 조합의 파라미터를 받는 split 확장 함수를 제공한다. 정규식을 파라미터로 받는 함수는 String이 아닌 Regex타입의 값을 받는다. 따라서 코틀린에서는 split 함수에 전달하는 값의 타입에 따라 정규식이나 일반 텍스트 중 어느 것으로 문자열을 분리하는지 쉽게 알 수 있다.

```kotlin
//마침표와 대시(-)로 문자열을 분리하는 예시
//정규식을 명시적으로 만든다.
println("12.345-6.A".split("\\.|-".toRegex()))
//결과 = [12, 345, 6, A]
```

간단한 경우에는 꼭 정규식을 쓸 필요가 없다. split 확장 함수를 오버로딩한 버전 중에는 구분 문자열을 하나 이상 인자로 받는 함수가 있다.

```kotlin
println("12.345-6.A".split(".", "-"))
//결과 = [12, 345, 6, A]
```

<br><br>

## <span style="color:orange">2. 정규식과 3중 따옴표로 묶은 문자열</span>
---
```kotlin
//String 확장 함수를 사용해 경로 파싱하기
fun parsePath(path: String) {
    val directory = path.substringBeforeLast("/")
    val fullName = path.substringAfterLast("/")
    val fileName = fullName.substringBeforeLast(".")
    val extension = fullName.substringAfterLast(".")
    println("Dir: $directory, name: $fileName, ext: $extension")
}
```
path에서 처음부터 마지막 슬래시 직전까지의 부분 문자열은 파일이 들어있는 디렉터리 경로다. path에서 마지막 마침표 다음부터 끝까지의 부분 문자열은 파일 확장자다. 파일 이름은 그 두 위치 사이에 있다.  
정규식을 사용하지 않고도 문자열을 쉽게 파싱할 수 있다. 정규식은 강력하지만 나중에 알아보기 힘든 경우가 많다. 정규식이 필요할 때는 코틀린 라이브러리를 사용하면 더 편하다.

```kotlin
//경로 파싱에 정규식 사용하기
fun parsePath(path: String) {
    val regex = """(.+)/(.+)\.(.+)""".toRegex()
    val matchResult = regex.matchEntire(path)
    if (matchResult != null) {
        val (directory, filename, extension) = matchResult.destructured
        println("Dir: $directory, name: $filename, ext: $extension")
    }
}
```

<br><br>

---

<br>

*참고 서적 : Kotlin IN Action*