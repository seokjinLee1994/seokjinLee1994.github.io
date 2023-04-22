---
layout: default
title: "[Kotlin] 코틀린 기초 열거형 클래스(enum class)와 when"
date: 2021-07-19 +0800
last_modified_at: 2021-07-19 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(004) 열거형 클래스(enum class)와 when

## <span style="color:orange">1. enum class</span>  
---  
enum은 소프트 키워드의 종류이다. 클래스 앞에 붙이면 열거형 클래스(enum class)가 된다.  
enum은 class 앞에 있을 때는 특별한 의미를 지니지만 다른 곳에서는 이름에 사용할 수 있다.  
프로퍼티나 메소드를 선언할 수 있다. 메소드를 선언하는 경우 enum 상수 목록 끝에 ;을 붙인다.  

```kotlin
//간단한 enum class 정의
enum class Color {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET
}
```

```kotlin
//프로퍼티와 메소드가 있는 enum 클래스 선언
enum class Color (val r: Int, val g: Int, val b: Int) {

    //각 상수를 생성할 때 그에 대한 프로퍼티 값을 지정한다. 반드시 끝에 ;을 사용
    RED(255, 0, 0), ORANGE(255, 165, 0), YELLOW(255, 255, 0),
    GREEN(0, 255, 0), BLUE(0, 0, 255), INDIGO(75, 0, 130), 
    VIOLET(238, 130, 238);

    //enum class 안에서 메소드를 정의
    fun rgb() = (r * 256 + g) * 256 + b
}
```

<br><br>

## <span style="color:orange">2. when으로 enum class 다루기</span>
---
자바의 switch문에 해당하는 코틀린의 구성요소.  
if와 마찬가지로 값을 만들어내는 식이다. 따라서 식이 본문인 함수에 when을 바로 사용 가능하다.  

```kotlin
//when을 사용해 원하는 enum 값을 찾는 간단한 함수
fun getMnemonic(color: Color) = 
    when (color) {
        Color.RED -> "Richard" 
        Color.ORANGE -> "Of" 
        Color.YELLOW -> "York" 
        Color.GREEN -> "Gave" 
        Color.BLUE -> "Battle" 
        Color.INDIGO -> "In" 
        Color.VIOLET -> "Vain"
    }
```
> _'Richard Of York Gave Battle In Vain'_  
> 각 단어의 첫 글자는 빨주노초파남보에 해당하는 형어 단어의 첫 글자다.  
> 무지개의 색을 기억하기 위해 연상법을 적용한 문장.  

자바와 달리 각 분기의 끝에 break를 넣지 않아도 된다.  
하나의 분기 안에서 여러 값을 매치 패턴으로 사용할 수도 있다. 그럴경우 ,(콤마)로 분리한다.  

```kotlin
//한 when 분기 안에 여러 값 사용하기
fun getWarmth(color: Color) = when (color) {
    Color.RED, Color.ORANGE, Color.YELLOW -> "warm" 
    Color.GREEN -> "neutral" 
    Color.BLUE, Color.INDIGO, Color.VIOLET -> "cold"
}
```
<br><br>

## <span style="color:orange">3. when과 임의의 객체를 함께 사용</span>
---
```kotlin
//when의 분기 조건에 여러 다른 객체 사용하기
fun mix(c1: Color, c2: Color) = 
    when (setOf(c1, c2)) {
        setOf(RED, YELLOW) -> ORANGE 
        setOf(YELLOW, BLUE) -> GREEN 
        setOf(BLUE, VIOLET) -> INDIGO 
        else -> throw Exception("Dirty color")
    }
```
> _setOf()_  
> 코틀린 표준 라이브러리에는 인자로 전달받은 여러 객체를 그 객체들을 포함하는 집합인 Set 객체로 만드는 setOf라는 함수가 있다.  
> 집합(set)은 원소가 모여있는 컬렉션으로, 각 원소의 순서는 중요하지 않다.  
> setOf(c1, c2)와 setOf(RED, YELLOW)가 같다는 말은 c1이 RED이고 c2가 YELLOW거나, 반대의 경우라는 말이다.
  
<br><br>

## <span style="color:orange">4. 인자 없는 when 사용</span>
---
```kotlin
fun mixOptimized(c1: Color, c2: Color) = 
    when {
        (c1 == RED && c2 == YELLOW) || 
        (c1 == YELLOW && c2 == RED) ->
            ORANGE 
        
        (c1 == YELLOW && c2 == BLUE) || 
        (c1 == BLUE && c2 == YELLOW) -> 
            GREEN 

        (c1 == BLUE && c2 == VIOLET) || 
        (c1 == VIOLET && c2 == BLUE) -> 
            INDIGO 
        
        else -> throw Exception("Dirty color")
    }
```
when에 아무 인자도 없으려먼 각 분기의 조건이 Boolean 결과를 계산하는 식이어야 한다.
추가 객체를 만들지 않는다는 장점이 있지만 setOf를 사용한 mix 함수보다는 가독성이 떨어진다.

<br>

---

<br>

*참고 서적 : Kotlin IN Action*