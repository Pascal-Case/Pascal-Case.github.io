---
layout: single
title: "[GitHub]Jekyll의 Dart Sass 전환 이슈 해결"
categories: "java"
tag: ["blog", "jekyll", "sass"]
---

# Jekyll의 Dart Sass 전환으로 인한 이슈 해결

## 이슈내용

현재 새롭게 시작하는 GitHub 블로그의 로컬환경에서의 테스트를 위해 Ruby와 jekyll의 최신버전을 설치하고 로컬서버에 내 블로그를 띄우면 다음과 같은 `Warning`이 발생한다. 서버 실행에는 문제가 없었기에 방치해 놓았는데 이번 기회에 해결해보고자 한다.

![image](https://github.com/Pascal-Case/Algorithm/assets/152583754/e08bfeb4-1ac5-4cc8-a90c-af4427788fb1)

에러 내용과 발생한 코드를 살펴보면 다음과 같다.

```
Deprecation Warning: Using / for division outside of calc() is deprecated and will be removed in Dart Sass 2.0.0.**
```

즉, 해당 CSS 코드의 나눗셈 계산을 위한 `/`가 `Dart Sass 2.0.0` 이후로는 `deprecated` 되었다 라는 뜻이다.

## 원인

`jekyll`의 `jekyll-sass-conterter`가 `3.x` 버전으로 올라가면서 Sass가 `Ruby Sass`에서 `Dart Sass`로 바뀜에 따라 기존 Sass와는 다른 문법이 적용되면서 `Warning`이 발생하였다.

> https://jekyllrb.com/news/2022/12/21/jekyll-sass-converter-3.0-released/

> https://sass-lang.com/documentation/breaking-changes/slash-div/

## 해결방법

해당 에러내용에 친절하게 **Recommedation**으로 해결방법을 알려주고 있다.

```
Recommendation: math.div(($red * 299) + ($green * 587) + ($blue * 114), 1000) or calc((($red * 299) + ($green * 587) + ($blue * 114)) / 1000)
```

CSS코드에 나눗셈을 할 경우 `/` 대신 해결방법에 나와있는 `math.div()` 나 `calc()` 중 하나로 바꿔주면 된다.

나는 첫 번째 방법으로 해결해 보고자 한다.

우선 `assets/css/main.css`에 `math모듈`을 사용하기 위한 다음과 같은 코드를 추가 해 준다.

![](https://github.com/Pascal-Case/Algorithm/assets/152583754/8b388ead-e1b0-4585-a968-f0b145a1c8f4)

`@use "sass:math";`를 `@import`문 보다 위에 작성해 주었다.

그리고 해당 `Warning`이 발생하는 `.scss`의 코드들을 `math.div({나눗셈 대상}, {나누고자 하는 값})` 형식으로 모두 바꿔주었다.

![](https://github.com/Pascal-Case/Algorithm/assets/152583754/d9e8024e-ee71-4d93-8bcb-f62d2ec5c9e8)

그리고 `jekyll` 서버를 다시 실행시켜 준다.

![](https://github.com/Pascal-Case/Algorithm/assets/152583754/ccbed8fa-68b7-481a-84c8-c53285b83100)

에러나 경고 없이 서버가 정상적으로 실행되었다.

이번에 마음먹고 해결하기 위해 에러 내용을 읽어보니 안에 원인과 해결방법이 모두 들어있었다. 에러가 발생하면 바로 쭉 긁어서 구글링하기 보단 에러코드 부터 확인하는 말이 괜히 하는 말이 아님을 다시 한번 느꼈다.
