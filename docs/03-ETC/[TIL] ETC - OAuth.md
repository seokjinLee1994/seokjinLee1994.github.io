---
layout: default
title: "[TIL] ETC - OAuth2.0"
date: 2024-05-19 +0800
last_modified_at: 2024-05-19 +0800
tags: [TIL ETC]
categories:
  - TIL
toc: true
parent: ETC
---

# [TIL] ETC - OAuth2.0

## **OAuth2.0**<br><br>

Open Authorization 2.0 혹은 OAuth2.0은 웹 및 애플리케이션 인증 및 권한 부여를 위한 개방형 표준 프로토콜이다. 이 프로토콜에서는 third-party 애플리케이션이 사용자의 리소스에 접근하기 위한 절차를 정의하고 서비스 제공자의 API를 사용할 수 있는 권한을 부여한다.

개발 중 흔히 마주칠 수 있는 요구사항인 구글, 페이스북, 카카오, 네이버 등 소셜 로그인(간편 로그인)에 사용되어지는 프로토콜이다.

<br>

---

## **OAuth2.0의 4가지 역할**<br><br>

OAuth2.0은 4가자의 역할로 구성되어 있다.<br>

1. Resource Owner(사용자)
    - 리소스 소유자. 우리의 서비스를 이용하면서 타사 플랫폼에서 리소스를 소유하고 있는 사용자.
    - 리소스의 예시 (구글 캘린더 정보, 페이스북 친구 목록, 네이버 블로그 포스트)
2. Client(우리 서비스 or 자사 플랫폼)
    - Resource Server의 자원을 이용하고자 하는 서비스
3. Resource Server
    - 타사 플랫폼이 리소스(사용자 정보)를 가지고 있는 서버
4. Authorization Server
    - Resource Owner를 인증, Client에게 Access Token을 발급해주는 서버

<br>

---

## **OAuth2.0 매커니즘**<br><br>
![OAuth2.0 매커니즘](../../img/OAuth/OAuth2.0_%20mechanism.png)

{: .note }
> **Client** <br><br>
위 스퀀스 다이어그램의 Client는 우리가 개발하는 서비스 자체를 말한다. 우리가 개발하는 서비스를 Client라고 정의한 이유는 Authotization & Resource Server 입장에서 클라이언트에 해당하기 때문이다.

<br>

### **1~2. 로그인 요청**
Resource Owner가 우리 서비스(Client)의 소셜 로그인(ex. 카카오로그인)의 로그인하기 등의 버튼을 클릭해 로그인을 요청한다.<br>
Client는 OAuth 프로세스를 시작하기 위해 Resource Owner의 브라우저를 Authorization Server로 보낸다.<br>
이때 Client는 response_type, client_id, redirect_uri, scope 등의 매개변수를 쿼리 스트링으로 포함하여 보낸다.<br>

- response_type: 반드시 code로 값을 설정해야 한다. 인증이 성공할 경우 Client는 후술할 Authorization Code를 받는다.
- client_id: 서비스를 Resource Server에 등록했을 때 발급 받은 Client ID를 의미한다.
- redirent_uri: 서비스를 Resource Server에 등록했을 때 등록한 redirect URI를 의미한다.
- scope: Client가 부여 받은 리소스 접근 권한
<br>

### **3~4. 로그인 페이지 제공, ID/PW 입력**
Client로 부터 Authorization URL로 이동된 Resource Owner는 제공된 로그인 페이지에서 ID/PW을 입력하여 인증을 한다.
<br>

### **5~6. Authorization Code 발급, Redirect URI로 리다이렉트**
Authorization URL에서 인증이 성공했다면, Authorization Server는 기존에 설정한 Redirect URI에 **Authorization Code**를 포함하여 사용자를 리다이렉션 시킨다.<br>

{: .note }
> **Authorization Code** <br><br>
리소스 접근을 위한 Access Token을 획득하기 위해 사용하는 임시 코드. 수명이 매우 짧다.

<br>

### **7~8. Authorization Code와 Access Token 발급**
Client는 다시 Authorization Server에 Authorization Code를 전달, Access Token을 발급 받는다.<br>
Client는 자신이 발급받은 Resource Owner의 Access Token을 DB에 저장하고, 이후 Resource Server에서 Resource Owner의 리소스에 접근하기 위해 Access Token을 사용한다.<br>

{: .warning }
>Access Token은 유출되지 않도록 관리해주어야 한다.

<br>

### **9. 로그인 성공**
위 과정을 모두 성공적으로 마치면 Client는 Resource Owner에게 로그인이 성공하였음을 알린다. 이제 Access Token으로 Resource Server에 접근 가능한 리소스를 요청할 수 있다.

<br>

### **10~13. 서비스 요청 및 Access Token을 이용하여 리소스 접근 및 이용**
Access Token을 발급 받았기 때문에 정해진 Scope 내에서 리소스를 이용할 수 있다.

<br>

---
## **Authorization Code가 필요한 이유**<br><br>

Access Token은 민감한 데이터다. 쉽게 노출되어서는 안된다.<br><br>
Redirect URI를 통해 Authorization Code를 발급하는 과정이 생략된다면, Authorization Server가 Access Token을 Client에 전달하기 위해 Redirect URI를 통해야 한다.<br>
Redirect URI를 통해 데이터를 전달할 방법은 URL 자체에 데이터를 실어 전달하는 방법 밖에 존재하지 않는다. 즉, 브라우저를 통해 데이터가 곧 바로 노출되게 된다.<br><br>
이런 보안 사고를 방지하기 위해 Authorization Code를 사용한다.

<br>

---

## **OAuth2.0의 스코프**<br><br>

OAuth2.0은 스코프라는 개념을 통해서 유저 리소스에 대한 클라이언트의 접근 범위를 제한 할 수 있다. 스코프는 여러개가 될 수 있고, 대소문자를 구분하는 문자열을 공백으로 구분하여 표현된다. 이때 문자열은 OAuth2.0 인증 서버에 의해 정의된다.
<br><br>
ex) 구글 로그인을 사용해서 로그인을 한 이후 사용자의 연락처를 받아오고 싶다면, scope에 연락처 scope 문자열을 포함하여 서버에 요청한다.
<br><br>
이러한 방식으로 발급된 Access Token은 Scope 정보를 가지고 있어서 권한을 제한한다.

<br>

---

## **OAuth2.0에서 백엔드와 프론트엔드**<br><br>

OAuth2.0 매커니즘의 시퀀스 다이어그램을 보면 알겠지만, 우리가 개발하는 서비스를 Client로 표시한다.<br>
Authotization & Resource Server 입장에서 우리의 서비스는 Client에 해당하기 때문이라고 말했는데, 결국 우리는 개발하는 과정에서 Client의 백엔드와 프론트엔드의 역할에 대해서 나누어야 한다.

- Authorization URL은 Client 백엔드가 생성한다.
- Client 프론트엔드는 해당 URL을 리다이렉트 시켜 브라우저가 인증을 가능하게 한다.
- 인증 결과로 받은 Authorization Code는 Client 프론트엔드에서 받는다.
- 프론트엔드는 Authorization Code를 백엔드로 보낸다.
- 백엔드에서 Authorization Code와 더불어 다양한 정보로 Authorization Server로 부터 Access Token을 발급 받는다.
- 백엔드는 발급 받은 Aceess Token을 프론트엔드에 제공하고, 프론트엔드는 백엔드로 부터 제공 받은 Access Token을 이용하여 Resource Owner의 리소스를 요청한다.

<br>

---

참고<br>
[OAuth2.0 개념과 동작원리](https://hudi.blog/oauth-2.0/)
<br>
[OAuth2.0이란 무엇일까?](https://velog.io/@choidongkuen/OAuth02-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)
<br>
[OAuth2.0 동작 방식의 이해](https://blog.naver.com/mds_datasecurity/222182943542)
<br>
[OAuth2.0 개념 및 연동](https://guide.ncloud-docs.com/docs/b2bpls-oauth2)