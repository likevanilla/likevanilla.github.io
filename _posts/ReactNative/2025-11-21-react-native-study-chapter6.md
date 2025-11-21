---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 4"
description: "[처음부터 배우는 리액트 네이티브] Chapter 6 Hooks"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-11-21
last_modified_at: 2025-11-21
---

[처음 배우는 리액트 네이티브] 책으로 리액트 네이티브 공부하기!

## 할 일 관리 Hooks

### useState

**useState** 함수는 호출을 하면 변수와 변수를 수정할 수 있는 세터 함수로 배열을 반환해요.
그리고 파라미터로 전할한 값을 초깃값으로 갖는 상태 변수와 그 변수를 수정할 수 있는 세터 함수로 배열을 반환해요.
또한 여러 번 사용할 수 있어요.

```jsx
// 예시 코드
const [state, setState] = useState(initialState);
```

#### 세터 함수

useState에서 반환된 세터 함수를 사용하는 방법은 두 가지가 있어요.

1. 세터 함수에 **변경될 상태의 값을 전달**하는 방법
2. 세터 함수의 **파라미터에 함수를 전달**하는 방법

```jsx
// 예시 코드
setState(preState => {});
```

전달된 함수에는 변경되기 전의 상태 값이 파라미터로 전달되며 이 값을 어떻게 수정할지 정의하면 돼요.

세터 함수는 **비동기로 동작**하기 때문에 상태 변경이 여러 번 일어날 경우 상태가 변경되기 전에 또 다시 상태에 대한 업데이트가 실행되는 상황이 발생해요.

##### 비동기 동작이란?

- 작업들의 시작/종료 시점 및 순서를 결정할 수 없는 상황
- 작업 주체의 작업 시작/종료 순간이 서로 일치하지 않는 상황

그래서 세터 함수를 호출해도 바로 상태가 변경되지 않는 문제가 발생할 수 있는데, 이럴 경우 세터 함수에 함수를 인자로 전달하여 **이전 상태값을 이용**하면 해결할 수 있어요.

```jsx
setCount(count + 1); // 상태가 변경되지 않음
setCount(prevCount => prevCount + 1); // 상태가 변경됨
```

### useEffect

**useEffect**는 컴포넌트가 렌더링될 때마다 원하는 작업이 실행되도록 설정할 수 있는 기능이에요.

```jsx
// 예시 코드
useEffect(() => {}, []);
```

useEffect의 첫 번째 파라미터로 전달된 함수는 **조건을 만족할 때마다 호출**되고, 두 번째 파라미터로 전달되는 배열을 이용해 **함수가 호출되는 조건**을 설정할 수 있어요.

useEffect의 두 번째 파라미터에 어떤 값도 전달하지 않으면 useEffect의 첫 번째 파라미터로 전달된 함수는 **컴포넌트가 렌더링될 때마다 호출**돼요.

#### 특정 조건에서 실행하기

useEffect에 설정한 함수를 특정 상태가 변경될 때만 호출하고 싶은 경우, useEffect의 두 번째 파라미터에 해당 상태를 관리하는 변수를 배열로 전달하면 돼요.

세터 함수는 비동기로 동작한다고 했죠? 그렇기 때문에 상태의 값이 변경되면 실행할 함수를 useEffect를 이용해서 정의하면 동기적으로 작동시킬 수 있어요.

#### 마운트될 때 실행하기

**마운트**는 가상 DOM에 렌더링된 컴포넌트가 실제 DOM으로 최초 커밋되는 것을 말해요.

useEffect의 **두 번째 파라미터에 빈 배열을 전달**하면 컴포넌트가 처음 렌더링될 때만 함수가 호출되게 해요.
이렇게 코드를 작성하면 조건을 "컴포넌트가 마운트될 때"로 설정할 수 있어요.

```jsx
useEffect(() => {
    console.log('\n===== Form Component Mount ====\n');
}, []);
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter6/log2.png?raw=true"
  style="width: 50%;"
/>

#### 언마운트될 때 실행하기

**언마운트**는 컴포넌트가 실제 DOM에서 제거되는 것을 말해요.

useEffect에서는 전달하는 함수에서 반환하는 함수를 **정리 함수**라고 하는데, useEffect의 **실행 조건이 빈 배열인 경우** 컴포넌트가 언마운트될 때 정리 함수를 실행시켜요.

```jsx
useEffect(() => {
    console.log('\n===== Form Component Mount ====\n');
    return () => console.log('\n===== Form Component UnMount ====\n');
}, []);
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter6/log3.png?raw=true"
  style="width: 50%;"
/>

### useRef

**useRef**는 저장공간(변수 관리), DOM 요소에 접근을 위해 사용이 돼요.

useRef를 사용할 때 주의해야 할 점이 두 가지 있어요.

1. 컴포넌트의 ref로 지정하면 생성된 변수에 값이 저장되는 것이 아니라 변수의 .current 프로퍼티에 해당 값을 담는 것
2. useState를 이용하여 생성된 상태와 달리 useRef의 내용이 변경돼도 컴포넌트는 다시 렌더링되지 않는다는 점

```jsx
// 예시코드
const ref = useRef(initialValue);

ref.current
```

### useMemo

**useMemo**는 동일한 연산의 반복 수행을 제거해서 성능을 최적화할 때 사용해요.

```jsx
// 예시 코드
useMemo(() => {}, []);
```

첫 번째 파라미터에는 함수를 전달하고, 두 번째 파라미터에는 함수 실행 조건을 배열로 전달하면 지정된 값에 변화가 있는 경우에만 함수가 호출돼요.

useMemo를 이용하면 계산하는 값에 변화가 있는 경우에만 함수가 호출되므로 중복되는 불필요한 연산을 제거할 수 있어요.

```jsx
const list = ['JavaScript', 'Expo', 'Expo', 'React Native'];

// useMemo로 최적화 전
const _onPress = () => {
    setLength(getLength(text));
    ++idx;
    if (idx < list.length) setText(list[idx]);
};

// 최적화 후
const _onPress = () => {
    ++idx;
    if (idx < list.length) setText(list[idx]);
};
const length = useMemo(() => getLength(text), [text]);
```

코드를 이렇게 최적화를 하면 동일한 문자열인 Expo를 다시 계산하지 않고, 배열의 마지막 값 이후에는 문자열의 변화가 없기 때문에 더 이상 getLength 함수를 호출하지 않아요.

특정 값에 변화가 있는 경우에만 함수를 실행하고, 값이 변하지 않으면 이전에 연산했던 결과를 이용해 중복된 연산을 방지하는 방식으로도 사용해서 성능을 최적화할 수 있어요.

### 커스텀 Hooks 만들기

리액트 네이티브에서는 네트워크 통신을 위해 **Fetch**와 **XMLHttpRequest**를 제공하고, 추가적으로 **WebSocket**도 지원해요.

교재에서는 useFetch라는 이름의 Hook을 만들어서 실습을 진행했어요.

Dogs API를 이용해서 무작위로 강아지 사진을 받아오는 컴포넌트를 만들었어요.

API 요청을 보내는 비동기 동작에서는 선행된 작업이 마무리되기 전에 추가적인 요청이 들어오지 않도록 화면을 구성하는 것이 좋다고 해요.

Hook도 컴포넌트와 마찬가지로 자주 사용되는 부분을 분리하면 코드가 깔끔해질 뿐만 아니라 여러 곳에서 재사용 가능하다는 장점이 있어요.

Hooks는 클래스형 컴포넌트를 사용하지 않아도 함수형 컴포넌트에서 상태를 관리하고 다양한 상황에 맞춰 작업할 수 있게 해주는 중요한 기능이에요.

>리액트를 배운지 반년이 넘어가고 있지만 커스텀 Hook을 만드는 것에 대해서는 아직 감이 잡히지 않는 것 같아요ㅜ.ㅜ 
>계속 리액트 프로젝트들을 진행하다보면 깨닫는 부분이 있겠죠?

다음 나오는 이미지들은 실습 결과를 저장한 사진들이에요. 블로그 파일에는 저장을 했는데 그냥 버리기는 아까워서 올려요ㅎㅎ

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter6/log1.png?raw=true"
  style="width: 50%;"
/>

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter6/simulator_screen1.png?raw=true"
  style="width: 50%;"
/>

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter6/simulator_screen2.png?raw=true"
  style="width: 50%;"
/>