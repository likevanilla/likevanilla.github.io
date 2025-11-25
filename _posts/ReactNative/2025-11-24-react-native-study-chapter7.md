---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 5"
description: "[처음부터 배우는 리액트 네이티브] Chapter 7 Context API"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-11-24
last_modified_at: 2025-11-25
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

리액트 네이티브 애플리케이션의 경우 데이터는 부모 컴포넌트에서 자식 컴포넌트로 전달돼요.

만약 데이터를 사용하는 컴포넌트가 많다면, 최상위 컴포넌트인 App 컴포넌트에서 상태를 관리하여 하위 컴포넌트 어디서 필요로 하든 전달할 수 있게 해야 해요.

하지만 이렇게 props를 이용하여 데이터를 전달하는 방법은 번거로워요.

### Context API

Context를 생성하는 **createContext 함수**는 파라미터에 생성되는 Context의 기본값을 지정할 수 있어요.

```jsx
// 예시 코드
const Context = createContext(defaultValue);
```

```jsx
// src/contexts/User.js

import { createContext } from 'react';

const UserContext = createContext({ name: "Jeonghyuk Lee"});

export default UserContext;
```

#### Consumer

생성된 Context 오브젝트는 입력된 기본값 외에도 **Consumer 컴포넌트**와 **Provider 컴포넌트**를 갖고 있어요.

**Consumer 컴포넌트**는 상위 컴포넌트 중 가장 가까운 곳에 있는 Provider 컴포넌트가 전달하는 데이터를 이용해요. 만약 상위 컴포넌트 중 Provider 컴포넌트가 없다면 createContext 함수의 파라미터로 전달된 기본값을 사용해요.

Consumer 컴포넌트의 자식은 반드시 리액트 컴포넌트를 반환하는 함수여야 해요.

```jsx
// src/components/User.js

import React from "react";
import styled from "styled-components";
import UserContext from "../contexts/User";

const StyledText = styled.Text`
  font-size: 24px;
  margin: 10px;
`;

const User = () => {
  return (

      <UserContext.Consumer>
        {(value) => <StyledText>Name: {value.name}</StyledText>}
      </UserContext.Consumer>
  );
};

export default User;
```

```jsx
// src/App.js
import React from "react";
import styled from "styled-components";
import User from "./components/User";

const Container = styled.View`
  flex: 1;
  background-color: #ffffff;
  justify-content: center;
  align-items: center;
`;

const App = () => {
  return (
      <Container>
        <User />
      </Container>
  );
};

export default App;
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter7/simulator_screen1.png?raw=true"
  style="width: 50%;"
/>

#### Provider

**Provider 컴포넌트**는 하위 컴포넌트에 Context의 변화를 알리는 역할을 해요.

Provider 컴포넌트는 value를 받아서 모든 하위 컴포넌트에 전달하고, 하위 컴포넌트는 Provider 컴포넌트의 value가 변경될 때마다 다시 렌더링해요. 그 전 까지는 undefined가 전달돼요.

```jsx
// src/App.js
...
import UserContext from "./contexts/User";
...
const App = () => {
  return (
    <UserContext.Provider>
      <Container>
        <User />
      </Container>
    </UserContext.Provider>
  );
};
...
```

그래서 이렇게 Provider 컴포넌트로 감싸기만 한다면 오류가 발생해요.

<img
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter7/simulator_screen2.png?raw=true"
    style="width: 50%;" 
/>

Provider 컴포넌트의 value에 값을 전달을 해야 요류가 나지 않아요.

```jsx
// src/App.js
...
const App = () => {
  return (
    <UserContext.Provider value={{ name: "Jeonghyuk Lee" }}>
      ...
    </UserContext.Provider>
  );
};
...
```

<img
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter7/simulator_screen2.png?raw=true"
    style="width: 50%;" 
/>

Provider 컴포넌트로부터 value를 전달받는 하위 컴포넌트의 수에는 제한이 없어요.

Consumer 컴포넌트는 가장 가까운 Provider 컴포넌트에서 값을 받으므로, 자식 컴포넌트 중 Provider 컴포넌트가 있다면 그 이후의 자식 컴포넌트는 중간에 있는 Provider 컴포넌트가 전달하는 값을 사용해요.

<img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter7/image1.png?raw=ture" style="width:50%;" 
/>

실습한 내용에서 Provider와 Consumer의 관계에서 결국 어떤 value가 출력이 되는지를 gemini pro에게 물어봐서 그림을 받아 왔어요.

App.js에 있는 Provider보다 User.js에 있는 Provider가 Consumer에게 더 가깝기 때문에 "React Native"라는 값을 출력하게 돼요.

Provider 컴포넌트를 사용할 때 반드시 value를 지정해야 한다는 점과 Consumer 컴포넌트는 가장 가까운 Provider 컴포넌트가 전달하는 값을 이용한다는 점을 알 수 있어요.

#### Context 수정하기