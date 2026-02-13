---
layout: post
title: "[Tailwind CSS] Tailwind CSS 정복하기 ! - 3"
description: "[Tailwind CSS로 만드는 멋진 웹 UI 스타일링] Chapter 13 ~ 17"

categories: tailwindcss
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2026-02-14
last_modified_at: 2026-02-14
---

* 목차
{:toc}

- aspect-video ⇒ 16:9 비율로 만들어줌

- aspect-square ⇒ 1:1 비율로 만들어줌

- aspect-[] ⇒ 대괄호 안에 넣으면 그 비율로 만들어줌 (예: 9/16)

---

## 인라인 요소

- 한 줄에 여러 개 배치
- 기본 너비 값은 컨텐츠의 너비 값
- 크기 값을 가질 수 없음
- 상하 마진은 가질 수 없음

## 블록 요소

- 한 줄에 한 개만 배치
- 기본 너비 값은 100%
- 크기 값을 가질 수 있음
- 상하좌우 마진 가질 수 있음

## 인라인 블록

- 한 줄에 여러 개 배치
- 기본 너비 값은 컨텐츠의 너비 값
- 크기 값을 가질 수 있음
- 상하좌우 마진을 가질 수 있음

- 블록 요소 중앙 정렬: 중앙 정렬이 될 자신에 margin: auto
- 인라인 요소 및 인라인 블록 요소 중앙 정렬: 부모 요소에 text-align 속성으로 정렬
- 모든 요소 왼쪽 배치 오른쪽 배치: float 속성으로 배치
- position: absolute 또는 position: fixed를 주면 모든 요소는 인라인 블록 요소로 display 속성이 변경됨

---

```html
      class="border-[5px] border-red-600 rounded-full overflow-hidden size-[200px]"
    >
      <img src="../../images/girl-face-01.jpg" />
    </div>
```

- 둥근 div border를 넘는 네모 이미지의 꼭짓점 부분을 overflow-hidden으로 없앨 수 있음 → 둥글게 됨

---

- 부모 요소에 relative를 적용해야 자식에 absolute를 적용했을 때 부모를 인식함
- 부모, 자식 둘 다 absolute를 적용해도 인식함

---

## 요소를 안보이게 하는 3가지 방법과 차이점

### hidden

- display: none
- 존재 자체가 사라지고 자기가 원래 갖고 있던 자리도 없어짐

### invisible

- visibility: hidden
- 존재는 사라지지만 자기가 원래 갖고 있던 자리는 유지

### opacity-0

- opacity: 0;
- 존재도 하고 자리도 유지 = 투명