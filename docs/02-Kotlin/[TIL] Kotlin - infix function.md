---
layout: default
title: "[TIL] Kotlin - infix function"
date: 2023-05-12 +0800
last_modified_at: 2023-05-12 +0800
tags: [TIL Kotlin]
categories:
  - TIL
toc:  true
parent: Kotlin
---

# [TIL] Kotlin - infix function

다중 선택: Kotlin


>💡 **infix function**<br>
	함수 선언에 사용되는 키워드로 해당 함수를 중위 표기법으로 호출할 수 있다.

<br>

infix function의 대표적인 예로 Pair를 만드는 to가 있다.

```kotlin
val pair : Pair<String, String> = "Hello" to "World"
```

직접 infix function을 만들어 보자

```kotlin
infix fun String.add(s: String) = this + s

val result = "Hello" add "World" // 결과 : HelloWorld
```

{: .note }
>infix fun 객체.함수명(매개 변수) : 리턴타입 { 구현 } 과 같은 형태로 만들 수 있다.

## 클래스 내에 infix function

```kotlin
class KeyboardSound {
	var sound = ""
	infix fun add(receiver: String) {
			this.sound += receiver
	}
}

fun main() {
	val keyboardSound = KeyboardSound()
		
	keyboardSound add "도각"
	keyboardSound add "도각"

	print(keyboardSound.sound) // 결과 : 도각도각
}
```

클래스 내부에 infix function을 정의한 경우, 객체는 자기 자신이기에 생략이 가능하다.

### 참고 자료

---

<[https://kotlinlang.org/docs/functions.html#infix-notation](https://kotlinlang.org/docs/functions.html#infix-notation)>

이펙티브 코틀린 - 마르친 모스칼라