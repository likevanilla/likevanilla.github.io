---
layout: post
title: "[Tailwind CSS] Tailwind CSS 정복하기 ! - 1"
description: "[Tailwind CSS로 만드는 멋진 웹 UI 스타일링] Chapter 1 ~ 7"

categories: tailwindcss
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2026-01-29
last_modified_at: 2026-01-29
---

* 목차
{:toc}

# Tailwind CSS를 시작한 이유

> 왜 Tailwind CSS를 배우는가?

이 질문에 대한 답을 먼저 해보자면..

정리한 내용에서도 나오겠지만 신속한 개발, 일관성 있는 디자인 시스템, 반응형 레이아웃 작업에 간편 등 여러가지 장점도 있고

요즘에는 Tailwind CSS가 추세라고 해서 배워두면 좋을 것 같다는 생각이 있었다.

그래서 대학교 동아리 **멋쟁이사자처럼**에서 강의를 하기로 이야기가 나왔고, 그렇기 때문에 공부를 시작했다.

이 뒤부터는 인프런에 있는 **Tailwind CSS로 만드는 멋진 웹 UI 스타일링** 강의를 보고 정리한 내용들로 채울 예정이다.

# Tailwind CSS의 장점과 단점

## 장점

- 신속한 개발로 개발 시간을 단축
- 클래스 네임을 고민할 필요 없음
- 일관성 있는 디자인 시스템을 적용하기 좋음
- 반응형 레이아웃 작업이 매우 간편
- 유지 관리 좋음
- 프레임워크의 독립성

## 단점

- 학습을 위해 시간이 필요함
- 익숙해지기 전까지 초반 높은 문서 의존도 높음
- 편리하고 빠르지만 지저분해 지는 HTML 코드로 코드 가독성이 많이 떨어짐
- 미세한 CSS 디자인에 제약
- CLI 설치 환경을 만들고 컴파일해야 하는 번거로움이 있음

# Tailwind CSS를 사용하기 전 해야되는 일
- Visual Studio Code에서 확장 프로그램으로 **Tailwind CSS IntelliSense**를 설치
- tailwindcss config 파일이 있어야 확장 프로그램을 사용할 수 있음
- Tailwind Config 파일은 반드시 루트(Root)에 만들어야 하고, 파일명도 반드시 `tailwind.config.js`로 만들어야 함

lorem → 의미없는 영단어 생성 (lorem10 → 단어 10개)

---
## 크기 단위 값

- REM 단위는 R(Root) 곧, html 또는 body에 설정되어 있는 폰트 사이즈를 기준으로 모든 것을 상대적(em)으로 결정되는 단위

## 색상 체계

- 색상 이름 22개, 색상 채도 11개
- 색상 코드, RGB를 대괄호에 넣어서 사용자 정의 가능
- rgba로 투명도 조절, 값들 사이에는 반드시 쉼표로 구분 + 공백x

## 방향에 관한 클래스 네임 체계

- padding
    - pt(top), pr(right), pb(bottom), pl(left)
    - px(left, right), py(top, bottom)

## emmet

- HTML/CSS 코드 작성을 획기적으로 줄여주는 플러그인
- div>div{0$}*10
    - div 밑에 div를 앞에 0이 있고 1부터 증가하는 숫자를 10개 만드는 명령어

```html
  <div>
    <div>01</div>
    <div>02</div>
    <div>03</div>
    <div>04</div>
    <div>05</div>
    <div>06</div>
    <div>07</div>
    <div>08</div>
    <div>09</div>
    <div>010</div>
  </div>
```

---

## 글자 서식 & 색상

**대괄호로 사용자 지정할 수 있음.**

- text-문자 : 글자 크기 (sm, lg, xl, 9xl)
- not-italic : 글자 기울임 안함
- font-[문자|숫자]: 글자 굵기 (thin, normal, bold | 100 ~ 900)
- tracking-문자 : 글자 간격 (tighter, wide)
- line-clamp-숫자 : 문단 끝 말 줄임 (1 ~ 6)
- leading-[숫자|문자] : 줄 간격 (3, 10, loose)
    - 가장 적절한 라인 하이트는 1.5REM or 1.6REM
- list-문자 : 목록 블릿(bullet) 표시하기 (disc(점), decimal(숫자), [square])
- lsit-inside: 목록 bullet을 안쪽으로 넣기
- list-outside: 목록 bullet을 바깥쪽으로 넣기
- list-image-[url(’’)] : 목록 블릿을 이미지로 표기하기