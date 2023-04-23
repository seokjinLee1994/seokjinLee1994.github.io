---
layout: default
title: "[TIL] ViewPager2의 setCurrentItem()이 동작하지 않을 때"
date: 2023-04-23 +0800
last_modified_at: 2023-04-23 +0800
tags: [TIL Android]
categories:
  - TIL
toc:  true
parent: Android
---
# [TIL] ViewPager2의 setCurrentItem()이 동작하지 않을 때

### 왜 움직이질 못하니 setCurrentItem()아…

최근 담당하고 있는 프로젝트의 viewpager를 viewpager2로 바꾸면서 어댑터 쪽의 코드를 수정하게 됐다.

분명 setCurrentItem()을 통해 시작 위치를 2번째 탭으로 지정해 두었는데 의도한대로 동작하지 않았다…

Why…?

원인은 viewpager의 아이템들이 뷰에 다 그려지기 전에 setCurrentItem()이 호출되어서 의도한대로 동작하지 않는 것으로 파악된다.

```kotlin
_binding.viewPager.post {
	_binding.viewPager.currentItem = 2
}
```

post 메소드를 이용하면 해결이 가능했다.

Android에서는 메인 스레드에서 UI 작업을 수행하기 때문에, 메인 스레드가 혼잡해지게 되면 사용자 인터페이스가 느려지더나 멈출 수 있다. 이를 방지하기 위해서 메인 스레드에서 실행되어야 하는 작업이 끝나면, 다음 작업을 예약하기 위해서 post 메소드를 사용했다.

{: .note }
<!-- ### post...? -->
View 클래스에서 상속받은 메소드로, UI 스레드에서 실행되는 작업을 예약하는데 사용된다.
인자로 전달된 코드 블록을 메인 스레드의 메시지 큐에 추가하고, 해당 코드 블록은 큐에서 순서대로 실행된다.

### 참고 자료

---

https://stackoverflow.com/questions/19316729/android-viewpager-setcurrentitem-not-working-after-onresume