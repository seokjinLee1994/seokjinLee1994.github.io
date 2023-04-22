---
layout: default
title: "[Kotlin] 코틀린 object 키워드"
date: 2021-08-23 +0800
last_modified_at: 2021-08-23 +0800
tags: [kotlin basic]
categories:
  - Kotlin Basic
toc:  true
---

# kotlin의 기본 문법 정리(024) 

## <span style="color:orange">1. object 키워드: 클래스 선언과 인스턴스 생성</span>  
---  

코틀린에서는 object 키워드를 다양한 상황에서 사용하지만 모든 경우 클래스를 정의하면서 동시에 인스턴스를 생성한다는 공통점이 있다. object 키워드를 사용하는 상황은 아래와 같다.

- 객체 선언(object declaration)은 싱글턴을 정의하는 방법 중 하나다.  
- 동반 객체(companion object)는 인스턴스 메소드는 아니지만 어떤 클래스와 관련 있는 메소드와 팩토리 메소드를 담을 때 쓰인다. 동반 객체 메소드에 접근할 때는 동반 객체가 포함된 클래스의 이름을 사용할 수 있다.  
- 객체 식은 자바의 무명 내부 클래스(anonymous inner class) 대신 쓰인다.

---

<br>

*참고 서적 : Kotlin IN Action*