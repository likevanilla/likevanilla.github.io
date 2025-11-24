---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 5"
description: "[처음부터 배우는 리액트 네이티브] Chapter 7 Context API"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-11-24
last_modified_at: 2025-11-24
---

* 목차
{:toc}

[처음 배우는 리액트 네이티브] 책으로 리액트 네이티브 공부하기!

## Context API

`Context API`는 데이터를 저역적으로 관리하고 사용할 수 있도록 하는 기능을 말해요.

이번에도 context에 대해 실습을 할 예정인데, 스터디를 같이 하는 사람들이 styled-components에서 에러가 많이 난다고 해요.

아무래도 책이 오래되기도 했고 그동안 기술들은 발전을 해서 버전이 안 맞는 것도 있을테니까요.

근데 저는 설치할 때 빼고는 따로 문제가 생기지는 않았어요!

제가 styled-components를 설치할 때는 항상 `npm i styled-components`를 입력해서 설치를 하거든요? 그러니 오류가 나시는 분들은 참고를 해주세용.

[오류 해결 방법](https://github.com/styled-components/styled-components/issues/5576)

정확히 같은 오류는 아닐 수 있겠지만 이슈 맨 아래에 해결 방법이 있다고 [스터디 팀원](https://velog.io/@choi-ss-choco/about)이 알려주더라구요. 이것도 따라해보면 좋을 것 같아용.

### 전역 상태 관리