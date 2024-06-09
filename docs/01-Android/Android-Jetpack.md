---
layout: default
title: "Android - Jetpack에 대해 알아보자"
date: 2024-06-08 +0800
last_modified_at: 2024-06-08 +0800
tags: [Android]
categories:
  - Android
toc:  true
parent: Android
---
# [TIL] Android Jetpack

## **Jetpack이 뭔가요...?**<br><br>

![Jetpack이 뭘까?](../../img/Jetpack/Jetpack%EC%9D%B4%EB%9E%80.png)
<https://developer.android.com/jetpack?hl=ko>

위는 안드로이드 개발자 페이지에서 소개하는 Jetpack에 대한 설명이다.<br>

위 설명과 같이 Jetpack이란 라이브러리의 묶음을 말한다.<br>

<br>

---

## **Jetpack 구조**<br><br>
![Jetpack 구조](../../img/Jetpack/Jetpack_%EA%B5%AC%EC%A1%B0.png)

Jetpack 라이브러리는 크게 4가지의 구성요소를 가진다.<br>
위 이미지만 봐도 알겠지만 Jetpack이란 Android 개발의 A to Z라고 볼 수 있다.<br>

<br>

---

## **Jetpack 등장 배경**<br><br>
![Jetpack 등장배경](../../img/Jetpack/Jetpack_%EB%93%B1%EC%9E%A5%EB%B0%B0%EA%B2%BD.jpeg)

이미 Android에는 Jetpack 이전에 support library가 존재했다.<br>
하지만 support library의 여러 문제점들이 존재했고 이를 개선하고, 다양한 버전을 통합 및 자체적으로 관리하는 새로운 네임스페이스 androidx를 구성했다.<br>

<br>

---

## **Jetpack을 사용하는 이유**<br><br>
![Jetpack 왜 사용해야 하나요?](../../img/Jetpack/Jetpack%EC%9D%84%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%20%EC%9D%B4%EC%9C%A0.png)

Android Developer 사이트를 보면 Jetpack을 사용해야 하는 이유에 대해서 위와 같이 설명해주고 있다.<br>

안드로이드 Jetpack의 장점중 하나는 이전 안드로이드 버전들과 호환이 된다는 것이다. Jetpack의 라이브러리들은 androidx.* 로 패키지화가 되어있기 때문에 API로부터 분리되어 있다. 또한 API와 분리되어 있고 라이브러리 형태이기 때문에 자주 업데이트가 되어 항상 가장 뛰어난 최신 버전의 Jetpack 구성요소에 액세스할 수 있다는 장점을 가진다.