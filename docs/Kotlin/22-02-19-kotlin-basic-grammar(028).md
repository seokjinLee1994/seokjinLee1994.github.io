---
layout: post
title: "[Kotlin] 동반 객체를 일반 객체처럼 사용"
date: 2022-02-19 +0800
last_modified_at: 2022-02-19 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(028) 

## <span style="color:orange">객체 식 : 무명 내부 클래스를 다른 방식으로 작성하기</span>  
---

object 키워드는 싱글턴과 같은 객체를 정의하고 그 객체에 이름을 붙일 때만 사용하지 않는다.<br>
무명 객체를<sup>[[1]](#footnote_1)</sup> <sup>[[2]](#footnote_2)</sup> 정의할 때도 object 키워드를 사용한다.<br>

```kotlin
// 무명 객체로 이벤트 리스너 구현하기
window.addMouseListener(
    object : MouseAdapter() {
        override fun mouseClicked (e: MouseEvent){
            ...
        }
        ...
    }
)
```

사용하는 구문은 객체 선언과 같다.<sup>[[3]](#footnote_3)</sup><br>
객체 식은 클래스를 정의하고 그 클래스에 속한 인스턴스를 생성하지만, 그 클래스나 인스턴스에 이름을 붙이지는 않는다.<br>
객체 선언과는 객체의 이름이 빠졌다는 점에서 다르다. 객체에 이름을 붙여야 하는 경우에는 무명 객체를 변수에 대입하여 사용한다.<br>

```kotlin
val listener = object : MouseAdapter() {
    ...
}
```

자바의 무명 클래스와 같이 객체 식 안의 콛드는 그 식이 포함된 함수의 변수에 접근할 수 있다.<br>
자바와는 달리 final이 아닌 변수도 객체 식 안에서 사용할 수 있다. 따라서 객체 식 안에서 그 변수의 값을 변경할 수 있다.<br>

```kotlin
// 무명 객체 안에서 로컬 변수 사용하기
fun countClicks(window : Window) {
    var clickCount = 0
    window.addMouseListener(object : MouseAdapter()) {
        override fun mouseClicked(e : MouseEvent) {
            clickCount++
        }
    }
}
```

객체 식은 무명 객체 안에서 여러 메소드를 오버라이드해야 하는 경우에 유용하다.<br>
메소드가 하나뿐인 인터페이스를<sup>[[4]](#footnote_4)</sup> 구현해야 한다면 코틀린의 SAM<sup>[[5]](#footnote_5)</sup><br>

---

<a name="footnote_1">1</a> : 무명 객체란 익명 클래스로부터 생성되는 객체를 뜻한다.<br>
<a name="footnote_2">2</a> : 무명 객체는 클래스가 한 번만 활용되어야 하는 경우에 매우 유용하다. 한 번만 활용되어야 하는데 매번 클래스를 작성시 클래스가 너무 많아지는 불편함이 존재한다.<br>
<a name="footnote_3">3</a> : 객체 선언과 달리 무명 객체는 싱글턴이 아니다. 객체 식이 쓰일 때마다 새로운 인스턴스가 생성된다.<br>
<a name="footnote_4">4</a> : Runnable 등의 인터페이스가 그렇다.<br>
<a name="footnote_5">5</a> : (샘)은 추상 메소드가 하나만 있는 인터페이스라는 뜻이다. 자바에서 Runnable, Comparator, Callable 등 상당수의 인터페이스가 SAM이다. 함수형 인터페이스라고도 부른다.<br>

*참고 서적 : Kotlin IN Action*