---
layout: default
title: "[TIL] unrecognized Attribute name MODULE (class com.sun.tools.javac.util.UnsharedNameTable$NameImpl)"
date: 2023-04-28 +0800
last_modified_at: 2023-04-28 +0800
tags: [TIL Android]
categories:
  - TIL
toc:  true
parent: Android
---

# [TIL] unrecognized Attribute name MODULE (class com.sun.tools.javac.util.UnsharedNameTable$NameImpl)

다중 선택: Android

{: . warning}
> [error] 
An exception has occurred in the compiler (1.8.0_345)…..
java.lang.AssertionError: annotationType(): unrecognized Attribute name MODULE

예전에 작업하던 프로젝트를 오랜만에 열어서 빌드를 실행했더니 해당 에러가 출력되며 빌드에 실패했다.

원인은 sdk 31 버전을 사용하고 있어서 생긴 에러였다.

File → Project Structure → SDK Location → Gradle Settings.

로 이동해서 Gradle JDK를 11 version으로 설정해주면 해결 가능하다.

### 참고 자료

---

<https://stackoverflow.com/questions/68344424/unrecognized-attribute-name-module-class-com-sun-tools-javac-util-sharednametab>