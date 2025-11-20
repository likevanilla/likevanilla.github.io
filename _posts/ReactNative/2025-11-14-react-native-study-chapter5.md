---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 3"
description: "[처음부터 배우는 리액트 네이티브] Chapter 5 할 일 관리 애플리케이션"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-11-17
last_modified_at: 2025-11-20
---

[처음 배우는 리액트 네이티브] 책으로 리액트 네이티브 공부하기!

## 할 일 관리 애플리케이션

이번 챕터에서는 앞에서 배운 내용들을 활용해서 할 일 관리 애플리케이션을 만들어볼 거에요.

즉 TODO 앱이죠!

### 프로젝트 준비하기

우선 프로젝트를 생성한 뒤에 스타일드 컴포넌트와 prop-types 라이브러리를 설치해요.

```terminal
expo init react-native-todo
cd react-native-todo
npm i styled-components prop-types
```

저는 이번에도 스타일드 컴포넌트와 prop-types가 설치가 되지 않아서 `--force`를 뒤에 붙여서 따로따로 설치를 해줬어요! 한 번에 해도 되니 저처럼 따로따로 하지 않으셔도 돼요ㅎㅎ

아래에 나오는 코드와 폴더 구조를 잘 따라해서 프로젝트 준비를 합시다!

> Chapter 1에서도 그랬지만 prop-types가 설치는 되지민

```jsx
// src/theme.js
export const theme = {
  background: "#101010",
  itemBackground: "#313131",
  main: "#778bdd",
  text: "#cfcfcf",
  done: "#616161",
};
```

```jsx
// src/App.js
import react from "react";
import styled, { ThemeProvider } from "styled-components/native";
import { theme } from "./theme";

const Container = styled.View`
  flex: 1;
  background-color: ${({ theme }) => theme.background};
  align-items: center;
  justify-content: center;
`;

export default function App() {
  return (
    <ThemeProvider theme={theme}>
      <Container></Container>
    </ThemeProvider>
  );
}
```

```jsx
// App.js
import App from './src/App';

export default App;
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter5/folder.png?raw=true"
  style="width: 50%;"
/>

다 따라하고 화면을 보면 검정색 화면이 보이게 될텐데, 그러면 잘 따라하신 거에요.

### 타이틀 만들기

타이틀을 만들어봅시다!

```jsx
const Container = styled.View`
  flex: 1;
  background-color: ${({ theme }) => theme.background};
  align-items: center;
  justify-content: flex-start;
`;

const Title = styled.Text`
  font-size: 40px;
  font-weight: 600;
  color: ${({ theme }) => theme.main};
  align-self: flex-start;
  margin: 0px 20px;
`;

export default function App() {
  return (
    <ThemeProvider theme={theme}>
      <Container>
        <Title>TODO List</Title>
      </Container>
    </ThemeProvider>
  );
}
```

이런식으로 코드를 작성하게 되면 왼쪽 상단에 `TODO List`라는 문구가 생기게 돼요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter5/simulator_screen1.png?raw=true"
  style="width: 50%;"
/>

#### SafeAreaView 컴포넌트

현재 iOS화면을 보면 타이틀이 노치나 다이나믹 아일랜드에 불편하게 가려져 있을 거에요.

그럴 때는 padding값을 부여해서 가려지지 않는 위치로 이동시킬 생각을 하게 되는데요.

리액트 네이티브에서는 자동으로 padding값이 적용되어 노치 디자인 문제를 해결할 수 있는 **SafeAreaView** 컴포넌트를 제공해요.

Container 컴포넌트가 View가 아닌 SafeAreaView 컴포넌트를 사용하도록 변경하면 타이틀이 가려지지 않고 잘 나오게 돼요.

확실히 모바일 개발 언어라 그런지 이런 것도 내장이 되어있네요+_+

#### StatusBar 컴포넌트

**StatusBar** 컴포넌트는 상태 바를 제어할 수 있는 컴포넌트를 말하는데요. 상태 바의 스타일을 변경할 수 있고, 안드로이드 기기에서 상태 바가 컴포넌트를 가리는 문제를 해결할 수 있어요.

이번에는 Render 부분에 코드를 작성하기 때문에 따로 import를 해줘야 해요.

```jsx
// src/App.js
...
import { StatusBar } from 'react-native';
...
<Container>
  <StatusBar
    barStyle="light-content"
    backgroundColor={theme.background}
  />
  <Title>TODO List</Title>
</Container>
...
```

위와 같이 코드를 수정하면 상태 바의 내용이 흰색으로 나오게 돼요.

참고로 `backgroundColor` 속성은 안드로이드에만 적용이 된답니다.

### Input 컴포넌트 만들기

리액트 네이티브에서는 Input 컴포넌트를 **TextInput** 컴포넌트로 불려요. 다른 타입들도 TextInput 컴포넌트로 받고 추후에 맞게 타입을 변경해주는 방법이 있다고 하네요.

```jsx
// src/components/Input.js
import React from "react";
import styled from "styled-components/native";

const StyledInput = styled.TextInput`
  width: 100%;
  height: 60px;
  margin: 3px 0;
  padding: 15px 20px;
  border-radius: 10px;
  background-color: ${({ theme }) => theme.itemBackground};
  font-size: 25px;
  color: ${({ theme }) => theme.text};
`;

const Input = () => {
  return <StyledInput />;
};

export default Input;
```

```jsx
// src/App.js
...
import Input from './components/Input'
...
<Container>
  ...
  <Input />
</Container>
...
```

위와 같이 코드를 작성하게 되면 Input 컴포넌트가 화면에 나타나게 돼요.

#### Dimensions

애플리케이션을 개발할 때면 다양한 크기의 기기들을 모두 고려하기는 어려워요. 그래서 리액트 네이티브에서는 크기가 다양한 기기를 대응하기 위해 현재 화면을 알 수 있는 **Dimensions**와 **useWindowDimensions**를 제공해요.

이 두 기능을 사용하면 현재 기기의 화면 크기를 알 수 있고, 그에 맞게 화면이 잘 나오도록 코드를 작성할 수 있어요.

**Dimensions**는 처음 값을 받아왔을 때의 크기로 고정되기 때문에 기기를 회전해서 화면이 전환되면 화면의 크기와 일치하지 않을 수 있어요. 이런 상황을 위해 이벤트 리스너를 등록하여 화면의 크기 변화에 대응할 수 있도록 기능을 제공해요.

**useWindowDimensions**는 리액트 네이티브에서 제공하는 Hooks 중 하나로, 화면의 크기가 변경되면 크기, 너비, 높이를 자동으로 업데이트 해줘요.

##### Dimensions 사용

```jsx
// src/components/Input.js
...
import { Dimensions } from 'react-native';

const StyledInput = styled.TextInput`
  width: ${({ width }) => width - 40}px;
  ...
`;

const Input = () => {
  const width = Dimensions.get('window').width;

  return <StyledInput width={width} />;
};
...
```

#### Input 컴포넌트

Input 컴포넌트에는 다양한 속성을 설정할 수 있는데, 먼저 placeholder를 적용을 해볼게요.

```jsx
// src/App.js
...
<Input placeholder="+ Add a Task" />
...
```

```jsx
// src/components/Input.js
...
const StyledInput = styled.TextInput.attrs(({ theme }) => ({
  placeholderTextColor: theme.main,
}))`...`;
...
const Input = ({ placeholder }) => {
  const width = Dimensions.get("window").width;

  return (
    <StyledInput width={width} placeholder={placeholder} maxLength={50} />
  );
};
...
```

코드를 설명하자면 props로 전달된 placeholder를 설정하고, 스타일드 컴포넌트의 attrs를 이용해서 theme에 정의된 색상을 placeholder의 색으로 설정했어요.
그리고 입력 가능한 글자의 수를 50자로 제한했어요.

`TextInput 컴포넌트`는 기본값으로 첫 글자가 대문자로 나타나고 오타 입력 시 수정을 제안하는 기능이 켜져 있어요. 그리고 iOS의 경우 키보드의 완료 버튼이 return으로 되어 있어요.

```jsx
// src/components/Input.js
    <StyledInput
      width={width}
      placeholder={placeholder}
      maxLength={50}
      autoCapitalize="none"
      autoCorrect={false}
      returnKeyType="done"
    />
```

이런식으로 속성을 추가해주면 대문자 전환, 수정 기능, 키보드 완료 버튼을 설정할 수 있어요.

`returnKeyType`에 설정할 수 있는 값 중에는 `done`이나 `next`처럼 두 플랫폼 모두 적용되는 값도 있지만 **iOS에만 적용되는** `none`이나 **안드로이드에만 적용되는** `join`같은 것도 있어요.

추가로 `keyboardAppearance`라는 속성도 있는데 이 속성으로 키보드의 색상을 변경할 수 있어요. 이 속성은 iOS에서만 적용이 돼요.
`keyboardAppearance="dark"`

#### 이벤트

