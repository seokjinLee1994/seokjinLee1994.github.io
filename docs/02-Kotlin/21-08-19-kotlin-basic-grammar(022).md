---
layout: default
title: "[Kotlin] 코틀린 기초 데이터 클래스"
date: 2021-08-19 +0800
last_modified_at: 2021-08-19 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
parent: Kotlin
---

# kotlin의 기본 문법 정리(022) 

## <span style="color:orange">1. 모든 클래스가 정의해야 하는 메소드 자동 생성</span>  
---  

```kotlin
// Client를 데이터 클래스로 선언하기
data class Client(val name: String, val postalCode: Int)
```

이제 Client 클래스는 자바에서 요구하는 모든 메소드를 포함한다.  
1. 인스턴스 간 비교를 위한 equals
2. HashMap과 같은 해시 기반 컨테이너에서 키로 사용할 수 있는 hashCode
3. 클래스의 각 필드를 선언 순서대로 표시하는 문자열 표현을 만들어주는 toString  

equals와 hashCode는 주 생성자에 나열된 모든 프로퍼티를 고려해 만들어진다. 생성된 equals 메소드는 모든 프로퍼티 값의 동등성을 확인한다. hashCode 메소드는 모든 프로퍼티의 해시 값을 바탕으로 계산한 해시 값을 반환한다. 이때 주 생성자 밖에 정의된 프로퍼티는 equals나 hashCode를 계산할 때 고려의 대상이 아니라는 사실에 유의하자.  

### _데이터 클래스와 불변성: copy() 메소드_  
데이터 클래스의 프로퍼티는 꼭 val일 필요는 없다.  
var 프로퍼티를 써도 되나 데이터 클래스의 모든 프로퍼티를 읽기 전용으로 만들어서 데이터 클래스응 불변 클래스로 만들라고 권장한다. HashMap 등의 컨테이너에 데이터 클래스 객체를 담는 경우엔 불변성이 필수적이다.  
데이터 클래스 인스턴스를 불변 객체로 더 쉽게 활용할 수 있게 코틀린 컴파일러는 copy 메소드를 제공한다. 객체를 메모리상에서 직접 바꾸는 대신 복사본을 만드는 편이 더 낫다. 복사본은 원본과 다른 생명주기를 가지며, 복사를 하면서 일부 프로퍼티 값을 바꾸거나 복사본을 제거해도 프로그램에서 원본을 참조하는 다른 부분에 전혀 영향을 끼치지 않는다.  

```kotlin
// Client 클래스에 copy 메소드를 직접 구현
...
    fun copy(name: String = this. name, postalCode: Int = this.postalCode) = Client(name, postalCode)
}
```

---

<br>

*참고 서적 : Kotlin IN Action*