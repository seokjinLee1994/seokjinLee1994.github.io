---
layout: default
title: "[Kotlin] 코틀린 기초 대상을 이터레이션(while, for)"
date: 2021-07-20 +0800
last_modified_at: 2021-07-20 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(005) 대상을 이터레이션(while, for)

## <span style="color:orange">1. 이터레이션 / while</span>  
---  
이터레이션은 결과를 생성하기위한 프로세스의 반복이다.  
시퀀스는 일부 끝점 또는 끝 값에 접근한다.  
프로세스의 각 반복은 단일 반복이며 각 반복의 결과는 다음 반복의 시작점이 된다. 
   
문장내에 컴퓨터 프로그래밍 내에 정의된 숫자만큼 반복되는 문장 블록을 말한다.  
그 문장 블록은 이터레이션 된다고 한다.  

```kotlin
//while과 do-while

//조건이 참인 동안 본문을 반복 실행
while (조건) {
    /*...*/
}

//처음에 무조건 본문을 한 번 실행 후 조건이 참인 동안 본문 반복 실행
do {
    /*...*/
} while (조건)
```

<br><br>

## <span style="color:orange">2. 범위와 수열</span>
---
범위는 기본적으로 두 값으로 이루어진 구간이다. 보통은 그 두 값은 정수 등의 숫자 타입의 값이며, .. 연산자로 시작 값과 끝 값을 연결해서 범위를 만든다.

```kotlin
val oneToTen = 1..10
```

코틀린의 범위는 폐구간 또는 양끝을 포함하는 구간이다.(위의 예시로 보자면 두 번째 값이 10이 항상 범위에 포함)  
어떤 범위에 속한 값을 일정한 순서로 이터레이션하는 경우를 수열`(progression)`이라 한다.

```kotlin
//피즈버즈 게임을 통한 범위와 수열의 사용 예시
fun fizzBuzz(i: Int) = when {
    i % 15 == 0 -> "FizzBuzz" 
    i % 3 == 0 -> "Fizz" 
    i % 5 == 0 -> "Buzz" 
    else -> "$i"
}

//1..100 범위의 정수에 대해 이터레이션한다.
for (i in 1..100) {
    print(fizzBuzz(i))
}
```

> _'FizzBuzz'_  
> 주어진 숫자에 대해 3으로 나눠지면 Fizz  
> 5로 나눠지면 Buzz  
> 3과 5 둘 다 만족하면 FizzBuzz라고 외치는 게임

```kotlin
//증가 값을 가지고 범위 이터레이션하기
for(i in 100 downTo 1 step 2) {
    print(fizzBuzz(i))
}

//until함수를 사용할 경우 끝 값인 100은 포함되지 않고 99까지만 포함된다.
for(i in 1 until 100) {
    print(fizzBuzz(i))
}
```
100 downTo 1은 역방향 수열을 만든다.(역방향 수열의 기본 증가값은 -1)  
step 2를 붙이면 증가 값의 절댓값이 2로 변경.
until 함수를 사용할 경우 끝 값은 포함되지 않은 반만 닫힌 범위에 이터레이션 할 수 있다.

<br><br>

## <span style="color:orange">3. 맵에 대한 이터레이션</span>
---
```kotlin
//맵을 초기화하고 이터레이션하기

//키에 대해 정렬하기 위해 TreeMap을 사용한다.
val binaryReps = TreeMap<Char, String>()

//A부터 F까지 문자의 범위를 사용해 이터레이션
for (c in 'A'..'F') {

    //아스키 코드를 2진 표현으로 변경
    val binary = Integer.toBinaryString(c.toInt())

    //c를 키로 c의 2진 표현을 맵에 넣는다
    binaryReps[c] = binary
}

//맵에 대해 이터레이션
//맵의 키와 값을 두 변수에 각각 대입한다
for ((letter, binary) in binaryReps) {
    println("$letter = $binary")
}

```
..연산자는 숫자 타입뿐 아니라 문자 타입의 값에도 적용 가능.  
get과 put을 사용하는 대신 map[key]나 map[key] = value를 사용해 값을 가져오고 설정이 가능하다.

```kotlin
//컬렉션을 이터레이션
val list = arrayListOf("10", "11", "1001")

//인덱스와 함께 컬레셔을 이터레이션
for ((index, element) in list.withIndex()) {
    println("$index: $element")
}
```
<br><br>

## <span style="color:orange">4. in으로 컬렉션이나 범위의 원소 검사</span>
---
```kotlin
//in을 사용해서 값이 범위에 속하는지 검사하는 코드
fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
fun isNotDigit(c: Char) = c !in '0'..'9'

println(isLetter('q'))
println(isNotDigit('x))
//결과는 둘 다 true가 나온다.
```

```kotlin
//when에서 in 사용하기
fun recognize (c: Char) = when (c) {
    in '0'..'9' -> "It's a digit!" 
    in 'a'..'z', in 'A'..'Z' -> "It's a letter!"
    else -> "I don't know"
}
println(recognize('8'))
```
<br>

---

<br>

*참고 서적 : Kotlin IN Action*