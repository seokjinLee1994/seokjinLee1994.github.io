---
layout: default
title: "Android - Jetpack Compose"
date: 2024-06-09 +0800
last_modified_at: 2024-06-09 +0800
tags: [Android]
categories:
  - Android
toc:  true
parent: Android
---
# Android Jetpack Compose

## **Jetpack Compose**<br>
![공식문서에서의 Compose 설명](../../img/Compose/Compose%EB%9E%80.png)

2021년 5월에 Android Studio Arctic Fox의 베타버전으로 처음 공개되었다.

네이티브 UI를 빌드하기 위한 Android 최신 도구 키트를 말한다.

선언형 UI 방식으로 기존의 xml을 사용한 명령형 UI 방식에서 획기적으로 코드 라인 수가 줄어들었으며, 코드의 재사용성을 높였다.

자, Compose가 선언형 UI인 것을 알았으니 선언형 UI가 무엇인지를 알아보자.

<br>

---

## **명령형 UI**<br>
`선언형 UI를 알기 전에 이와 반대되는 명령형 UI에 대해서 먼저 알아보자.`<br>
기존 xml을 사용한 UI 구성 방법은 명령형 UI다.<br>
명령형 UI는 UI를 **"어떻게"** 구성할 것인지에 대해 묘사한다. 보통 UI라고 하면, UI의 상태(state)가 있고 이에 따라 UI가 갱신된다. 이때 명령형 UI는 상태값 변화에 대한 이벤트를 받아서 각 이벤트마다 적절한 UI를 업데이트 해야 한다.<br>
**즉, 상태값이 많아질수록 받아야 하는 이벤트도 증가하고 그에 따라 코드도 길어지고 관리 비용도 커진다.**

명령형 UI를 사용해 "Hello Android"를 표시하는 TextView를 만든다면 아래와 같다.

```xml
<TextView
    android:id="@+id/tvTitle"
    android:text="Hell Android"
    ...
/>
```
```kotlin
// xml 내에서 뿐만 아니라 Activity or Fragment 등 다양한 곳에서 접근 가능하다.
val textView = findViewById<TextView>(R.id.tvTitle)
textView.text = "Hello Android"
```

TextView는 Android에서 제공해주는 API 객체이며 아래와 같은 형태를 가지고 있다.
```java
@RemoteView
public class TextView extends View implements ViewTreeObserver.OnPreDrawListener { 
    ...
    @android.view.RemotableViewMethod
    public final void setText(CharSequence text) {
        setText(text, mBufferType);
    }
    ...
 }
```

xml 내에서 TextView에 `android:text="Hello Android"`는 TextView class 내의 setText() 함수를 호출한다. 그리고 text 상태값을 보유하고 있기 위해서 메모리 내에는 TextView 객체가 존재하게 된다.
<br>

좀 더 자세한 설명은 [여기](https://developer.android.com/develop/ui/compose/mental-model?hl=ko&continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fcompose%3Fhl%3Dko%23article-https%3A%2F%2Fdeveloper.android.com%2Fjetpack%2Fcompose%2Fmental-model#paradigm)를 참조하면 좋을거 같다.

<br>

>### **`💡결론`**<br>
>1. 기존 xml을 사용한 UI 구성 방식은 "어떻게" 구성할 것인지 묘사하는 명령형 방식이다.<br>
>2. 상태값 변화에 대한 이벤트를 받아서 각 이벤트마다 적절한 UI를 업데이트 해야 한다.
>3. 상태값이 많아질수록 받아야 하는 이벤트도 증가하고 그에 따라 코드도 길어지고 관리 비용도 커진다.

<br>

---

## **선언형 UI**<br>
Compose는 **`선언형 UI`** 다.<br>
선언형 UI는 상태와 함께 **'무엇을'** 렌더링 할지 정의해주면 자세한 부분은 프레임워크에서 처리해준다.<br>
**명령형 UI와는 다르게 선언형 UI는 상태값 변화에 따라서 UI를 통째로 다시 그린다. 이 부분은 프레임워크에서 처리해주기 때문에 코드가 짧고 관리 비용이 추가로 들지 않는다.**

명령형 UI를 설명하며 "Hello Android"를 보여주는 TextView를 예시로 들었는데, Compose로 만들면 아래와 같다.

```kotlin
@Composable
fun LoadTextView() {
    Text(text = "Hello Android")
}
```

xml을 사용할 때와 비교해 보면 코드라인이 줄어든걸 알 수 있다.

개인적으로 이런 Compose의 장점은 RecyclerView를 사용할 때 아주 격하게 체감이 되었다.
xml을 사용하여 RecyclerView를 사용할 때면 아래와 같은 작업을 해주어야 했다.
- RecyclerView의 아이템(데이터)를 그려주기 위한 xml
- RecyclerView와 데이터를 bind하는 역할의 Adapter class
- 각 위치에 대한 뷰와 데이터를 표현하는 ViewHolder class
- Activity 또는 Fragment에서 RecyclerView와 Adapter class의 연결

하지만 Compose에선 LazyColumn() 또는 LazyRow()를 사용하면 너무나 편하고 쉽게 구현 가능하다.

```kotlin
@Composable
fun ListView(itemList: List<String>) {
    LazyColumn {
        items(items = itemList) { item ->
            ItemView(item = item)
        }
    }
}

@Composable
fun ItemView(item: String) {
    Text(text = item)
}
```
**`이걸로 끝이다.`**

좀 더 자세한 부분은 [여기](https://developer.android.com/develop/ui/compose/mental-model?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fcompose%3Fhl%3Dko%23article-https%3A%2F%2Fdeveloper.android.com%2Fjetpack%2Fcompose%2Fmental-model#skips)를 참고하면 된다.
<br>
<br>

{: .note }
> **Compose는 객체인가...?** <br><br>
Compose를 사용해 보면 Text(), LazyColumn() 등 첫 글자가 대문자라 객체라 착각 할 수도 있다. 이는 Compose의 컨벤션일 뿐이다.
```kotlin
@Composable
fun Text(
    text: String,
    modifier: Modifier = Modifier,
    color: Color = Color.Unspecified,
    fontSize: TextUnit = TextUnit.Unspecified,
    fontStyle: FontStyle? = null,
    fontWeight: FontWeight? = null,
    fontFamily: FontFamily? = null,
    letterSpacing: TextUnit = TextUnit.Unspecified,
    textDecoration: TextDecoration? = null,
    textAlign: TextAlign? = null,
    lineHeight: TextUnit = TextUnit.Unspecified,
    overflow: TextOverflow = TextOverflow.Clip,
    softWrap: Boolean = true,
    maxLines: Int = Int.MAX_VALUE,
    minLines: Int = 1,
    onTextLayout: ((TextLayoutResult) -> Unit)? = null,
    style: TextStyle = LocalTextStyle.current
) { … }
```

>### **`💡결론`**<br>
>1. Compose는 "무엇을" 렌더링 할지 정의하는 선언형 UI다.<br>
>2. 상태값 변화에 따라서 UI를 통째로 다시 그린다.
>3. 자세한 부분은 프레임워크에서 알아서 처리해주기 때문에 코드가 짧고 관리 비용이 추가로 들지 않는다.