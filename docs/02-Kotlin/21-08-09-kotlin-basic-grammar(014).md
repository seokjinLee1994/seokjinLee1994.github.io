---
layout: default
title: "[Kotlin] 코틀린 기초 가시성 변경자 "
date: 2021-08-09 +0800
last_modified_at: 2021-08-09 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(014) 

## <span style="color:orange">1. 가시성 변경자: 기본적으로 공개</span>  
---  

변경자 | 클래스 멤버 | 최상위 선언
---| ---| ---|
public(기본 가시성) | 모든 곳에서 볼 수 있다. | 모든 곳에서 볼 수 있다.
internal | 같은 모듈 안에서만 볼 수 있다. | 같은 모듈 안에서만 볼 수 있다.
protected | 하위 클래스 안에서만 볼 수 있다. | (최상위 선언에 적용할 수 없음)
private | 같은 클래스 안에서만 볼 수 있다. | 같은 파일 안에서만 볼 수 있다.

코틀린의 기본 가시성은 자바와 다르다. 아무 변경자도 없는 경우 모두 공개(public)된다.  
자바의 패키지 전용 가시성에 대한 대안으로 코틀린에는 새로운 internal이라는 새로운 가시성 변경자를 도입했다. internal은 "모듈 내부에서만 볼 수 있음"이라는 뜻이다. 모듈은 한 번에 한꺼번에 컴파일되는 코틀린 파일들을 의미한다. InteliJ나 이클립스, 메이븐, 그레이들 등의 프로젝트가 모듈이 될 수 있고, 앤트 태스크가 한 번 실행될 때 함께 컴파일되는 파일의 집합도 모듈이 될 수 있다.  


```kotlin
// giveSpeech 함수 안의 각 줄은 가시성 규칙을 위한반다. 컴파일 시 오류 발생
internal open class TalkativeButton: Foucusable {
    private fun yell() = println("Hey!")
    protected fun whisper() = println("Let's talk!")
}

// 오류: "public" 멤버가 자신의 "internal" 수신 타입인 "TalkativeButton"을 노출함
fun TalkativeButton.giveSpeech() {

    // 오류: "yell"에 접근할 수 없음: "yell"은 "TalkativeButton"의 "private" 멤버임
    yell()

    // 오류: "whisper"에 접근할 수 없음: "whisper"는 "TalkativeButton"의 "protected" 멤버임
    whisper()
}
```

코틀린은 public 함수인 giveSpeech 안에서 그보다 가시성이 더 낮은 (이 경우 internal) 타입인 TalkativeButton을 참조하지 못하게 한다. 이는 어떤 클래스의 기반 타입 목록에 들어있는 타입이나 제네릭 클래스의 타입 파라미터에 들어있는 타입의 가시성은 그 클래스 자신의 가시성과 같거나 더 높아야하고, 메소드의 시그니처에 사용된 모든 타입의 가시성은 그 메소드의 가시성과 같거나 더 높아야 한다는 더 일반적은 규칙에 해당한다. 이런 규칙은 어떤 함수를 호출하거나 어떤 클래스를 확장할 때 필요한 모든 타입에 접근할 수 있게 보장해준다. 여기서 컴파일 오류를 없애려면 giveSpeech 확장 함수의 가시성을 internal로 바꾸거나, TalkativeButton 클래스의 가시성을 public으로 바꿔야 한다.

---

<br>

*참고 서적 : Kotlin IN Action*