---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 2"
description: "[처음부터 배우는 리액트 네이티브] Chapter 4 스타일링"

categories: ReactNative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-11-14
last_modified_at: 2025-11-14
---

## 스타일링

### 스타일링

#### 인라인 스타일링

**인라인 스타일**은 HTML의 인라인 스타일링처럼 컴포넌트에 직접 스타일을 입력하는 방식을 말해요.

리액트 네이티브에서는 **객체 형태**로 전달해야 한다는 점이 HTML과의 차이점!

인라인 스타일링은 어떤 스타일이 적용되는지 잘 보인다는 장점이 있어요.

하지만 비슷한 역할을 하는 컴포넌트에 동일한 코드가 반복된다는 점과, 어떤 이유로 해당 스타일이 적용 되었는지 코드만으로는 명확하게 이해하기 어렵다는 단점이 있어요.

```jsx
// src/App.js

import React from "react";
import { View, Text } from "react-native";

const App = () => {
  return (
    <View
      style={{
        flex: 1,
        backgroundColor: "#fff",
        alignItems: "center",
        justifyContent: "center",
      }}
    >
      <Text
        style={{
          padding: 10,
          fontSize: 26,
          fontWeight: "600",
          color: "black",
        }}
      >
        Inline Styling - Text
      </Text>
      <Text
        style={{
          padding: 10,
          fontSize: 26,
          fontWeight: "400",
          color: "red",
        }}
      >
        Inline Styling - Error{" "}
      </Text>
    </View>
  );
};
export default App;

```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/simulator_screen1.png?raw=true"
  style="width: 50%;"
/>

#### 클래스 스타일링

인라인 스타일링의 단점을 보완하기 위한 방법 중 하나로 **클래스 스타일링**이 있어요.

클래스 스타일링은 컴포넌트의 태그에 직접 입력하는 방식이 아니라 스타일시트에 정의된 스타일을 사용하는 방법이에요.

웹 프로그래밍에서 CSS 클래스를 이용하는 방법과 유사해요.

```jsx
// src/App.js

import React from "react";
import { StyleSheet, View, Text } from "react-native";

const App = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Inline Styling - Text</Text>
      <Text style={styles.error}>Inline Styling - Error</Text>
    </View>
  );
};
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center",
  },
  text: {
    padding: 10,
    fontSize: 26,
    fontWeight: "600",
    color: "black",
  },
  error: {
    padding: 10,
    fontSize: 26,
    fontWeight: "400",
    color: "red",
  },
});

export default App;
```

보이는 화면은 동일하지만 코드는 변경되었어요.

코드가 조금 더 깔끔해졌죠?

간단하게 화면을 확인하는 상황에서는 인라인 스타일이 편하지만, 장기적으로 생각하면 클래스 스타일을 사용하는 것이 유리해요.

#### 여러 개의 스타일 적용

여기서 error와 text의 중복된 스타일을 줄이고 스타일 적용을 여러 개 해주는 방법으로 스타일 코드를 더 간결하게 만들 수 있습니다.

```jsx
// src/App.js
    ...

<Text style={[styles.text, styles.error]}>Inline Styling - Error</Text>

    ...

  text: {
    padding: 10,
    fontSize: 26,
    fontWeight: "600",
    color: "black",
  },
  error: {
    fontWeight: "400",
    color: "red",
  },

    ...
```

마치 스타일을 리스트 형식으로 여러 개 적용시키는 느낌이 나네요.

**참고로 뒤에 오는 스타일이 앞에 있는 스타일을 덮는다는 것을 기억해야 해요.**

만약 `[styles.error, styles.text]`로 작성한다면 글자 색이 검정색으로 변해요.

여러 개의 스타일을 적용할 때 항상 클래스 스타일만 적용하는 것이 아닌 인라인 스타일과 혼용해서 사용할 수도 있어요.

`<Text style={[style.text, { color: 'green'}]}>Inline Styling - Text </Text>`

이렇게 하면 글자 색이 초록색이 되겠죠?

#### 외부 스타일 이용하기

여러 개의 파일에서 스타일을 공통으로 사용하고 싶을 때 따로 스타일 파일을 만들고, 필요할 때 가져와서 사용할 수 있습니다.

```jsx
// src/styles.js
import { StyleSheet } from "react-native";

export const viewStyles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center",
  },
});

export const textStyles = StyleSheet.create({
  text: {
    padding: 10,
    fontSize: 26,
    fontWeight: "600",
    color: "black",
  },

  error: {
    fontWeight: "400",
    color: "red",
  },
});
```

```jsx
// src/App.js
import React from "react";
import { View, Text } from "react-native";
import { viewStyles, textStyles } from "./styles";

const App = () => {
  return (
    <View style={viewStyles.container}>
      <Text style={[textStyles.text, { color: "green" }]}>
        Inline Styling - Text
      </Text>
      <Text style={[textStyles.text, textStyles.error]}>
        Inline Styling - Error
      </Text>
    </View>
  );
};

export default App;
```

### 리액트 네이티브 스타일

#### flex와 범위

화면을 구성할 때 Header, Contents, Footer 등으로 구성을 하게 되는데, 이 범위를 고정값으로 정하게 되면 이용하는 기기마다 화면 크기의 차이 때문에 서로 다른 모습으로 나타나고 이런 다양한 크기의 기기에 대응하기 어려워요.

그래서 flex를 사용하게 되는데, **flex**는 width나 height와 달리 항상 비율로 크기가 설정돼요.

flex는 값으로 숫자를 받으며 값이 0일 때는 설정된 width와 height값에 따라 크기가 결정되고, 양수인 경우 flex값에 비례하여 크기가 조정돼요.

```jsx
import React from "react";
import { StyleSheet, View, Text } from "react-native";

export const Header = () => {
  return (
    <View style={[style.container, style.header]}>
      <Text style={style.text}>Header</Text>
    </View>
  );
};

export const Contents = () => {
  return (
    <View style={[style.container, style.contents]}>
      <Text style={style.text}>Contents</Text>
    </View>
  );
};

export const Footer = () => {
  return (
    <View style={[style.container, style.footer]}>
      <Text style={style.text}>Footer</Text>
    </View>
  );
};

const style = StyleSheet.create({
  container: {
    width: "100%",
    alignItems: "center",
    justifyContent: "center",
    height: 80,
  },
  header: {
    flex: 1,
    backgroundColor: "#f1c40f",
  },
  contents: {
    flex: 2,
    backgroundColor: "#1abc9c",
  },
  footer: {
    flex: 1,
    backgroundColor: "#3498db",
  },
  text: {
    fontSize: 26,
  },
});
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/simulator_screen2.png?raw=true"
  style="width: 50%;"
/>

이렇게 각각 header, contents, footer에 flex 값을 넣게 되면, 화면 비율에 맞게 잘 구성이 돼요.

#### 정렬

##### flexDirection

지금까지 실습했던 것을 되돌아보면 컴포넌트들이 아래로 쌓인다는 것을 알 수 있는데요.

**flexDirection**을 이용하면 컴포넌트가 쌓이는 방향을 변경할 수 있어요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/flexDirection.png?raw=true"
  style="width: 50%;"
/>

- **column**: 세로 방향으로 정렬(기본값)
- **column-reverse**: 세로 방향 역순 정렬
- **row**: 가로 방향으로 정렬
- **row-reverse**: 가로 방향 역순 정렬

알아야할 점은 flexDirection은 자기 자신이 쌓이는 방향이 아니라 **자식 컴포넌트가 쌓이는 방향**이에요.

##### justifyContent

컴포넌트를 방향에 따라 정렬하는 방식을 결정하는 속성이 justifyContent와 alignItem에요.

**justifyContent**는 flexDirection에서 결정한 방향과 **동일한 방향으로 정렬**하는 속성이고,
**alignItems**는 flexDirection에서 결정한 방향과 **수직인 방향으로 정렬**하는 속성이에요.

###### justifyContent 설정 값

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/justifyContent.png?raw=true"
  style="width: 50%;"
/>

- flex-start: 시작점에서부터 정렬(기본값)
- flex-end: 끝에서부터 정렬
- center: 중앙 정렬
- space-between: 컴포넌트 사이의 공간을 동일하게 만들어서 정렬
- space-around: 컴포넌트 각각의 주변 공간을 동일하게 만들어서 정렬
- space-evenly: 컴포넌트 사이와 양 끝에 동일한 공간을 만들어서 정렬

##### alignItems 설정 값

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/alignItems.png?raw=true"
  style="width: 50%;"
/>

- flex-start: 시작점에서부터 정렬(기본값)
- flex-end: 끝에서부터 정렬
- center: 중앙 정렬
- stretch: alignItems의 방향으로 컴포넌트 확장
- baseline: 컴포넌트 내부의 텍스트(text) 베이스라인(baseline)을 기준으로 정렬

#### 그림자

##### 그림자 설정 값

- shadowColor: 그림자 색 설정
- shadowOffset: width와 height값을 지정하여 그림자 거리 설정
- shadowOpacity: 그림자의 불투명도 설정
- shadowRadius: 그림자의 흐림 반경 설정

이 속성들은 iOS에서는 적용되지만 안드로이드에서 그림자를 표현하려면 **elevation**이라는 속성을 사용해야 해요.

그래서 이 점을 보완하기 위해 리액트 네이티브에서 제공하는 **Platform 모듈**을 이용하면 각 플랫폼마다 다른 코드가 적용되도록 코드를 작성할 수 있어요.

```jsx
// src/components/ShadowBox.js

import React from "react";
import { StyleSheet, View, Platform } from "react-native";

export default () => {
  return <View style={styles.shadow}></View>;
};

const styles = StyleSheet.create({
  shadow: {
    backgroundColor: "#fff",
    width: 200,
    height: 200,
    ...Platform.select({
      ios: {
        shadowColor: "#000",
        shadowOffset: {
          width: 10,
          height: 10,
        },
        shadowOpacity: 0.5,
        shadowRadius: 10,
      },
      android: {
        elevation: 20,
      },
    }),
  },
});

```

이런식으로 iOS와 안드로이드에서 스타일 코드가 다르게 적용되도록 작성할 수 있어요.

하지만 양쪽 모두의 속성을 알아야하고 두 번 작성을 해야되니 번거로워 보이네요.

### 스타일드 컴포넌트

**스타일드 컴포넌트**는 자바스크립트 파일 안에 스타일을 작성하는 CSS-in-JS 라이브러리며, 스타일이 적용된 컴포넌트라고 생각하면 이해하기 쉬워요.

스타일드 컴포넌트를 사용하느 방법은 "styled.컴포넌트 이름"형태 뒤에 백틱( ` ) 을사용하여 만든 문자열을 붙이고 그 안에 스타일을 지정하면 돼요.

우선 터미널에 다음 명령어를 입력해서 스타일드 컴포넌트를 설치합니다.

`npm install styled-components`

저는 왜인지 에러가 떠서 뒤에 `--force`를 추가해서 강제로 설치했어요.

```jsx
import styled from 'styled-components/native';

const MyTextComponent = styled.Text`
    color: #fff;
    `;
```


이 문법을 **태그드 템플릿 리터럴**이라고 해요

스타일드 컴포넌트를 사용할 때 주의할 점은 styled 뒤 컴포넌트의 이름은 반드시 존재하는 컴포넌트를 지정해야 한다는 것이에요.

스타일드 컴포넌트에서는 css를 이용하여 재사용 가능한 코드를 관리할 수 있어요.

```jsx
// 예시 코드
import styled, { css } from 'styled-components/native';

const whiteText = css`
    color: #fff;
    font-size: 14px;
`;

const MyBoldTextComponent = styled.Text`
    ${whiteText}
    font-weight: 600;
`;

const MyLightTextComponent = styled.Text`
    ${whiteText}
    font-weight: 200;
`;
```

또는 완성된 컴포넌트를 상속받아 이용하는 방법도 있어요.

```jsx
import styled from 'styled-components/native';

const StyledText = styled.Text`
    color: #000;
    font-size: 20px;
    margin: 10px;
    padding: 10px
`;

const ErrorText = styled(StyledText)`
    font-weight: 600;
    color: red;
`;
```

스타일드 컴포넌트를 사용하니 규칙을 잘 모르겠던 숫자에 ''를 씌우거나 안 씌우거나 하는 부분이 없고, 속성이 기존에 쓰던 이름과 같아서 오타가 더 줄어서 편한 것 같다!

스타일드 컴포넌트에서 이미 작성된 스타일을 상속받아 새로운 스타일드 컴포넌트를 만들 때는 기존에 사용하던 styled.컴포넌트 이름 문법이 아니라 styled(컴포넌트 이름)처럼 **소괄호**로 감싸야 한다는 점을 주의해야 해요.

#### 스타일 적용하기

스타일 적용하기 실습이에요.

```jsx
// src/components/Button.js

import React from "react";
import styled from "styled-components/native";

const ButtonContainer = styled.TouchableOpacity`
  background-color: #9b59b6;
  border-radius: 15px;
  padding: 15px 40px;
  margin: 10px 0px;
  justify-content: center;
`;
const Title = styled.Text`
  font-size: 20px;
  font-weight: 600;
  color: #fff;
`;

const Button = (props) => {
  return (
    <ButtonContainer>
      <Title>{props.title}</Title>
    </ButtonContainer>
  );
};

export default Button;
```

```jsx
// src/App.js
import React from "react";
import styled from "styled-components/native";
import Button from "./components/Button";

const Container = styled.View`
  flex: 1;
  background-color: #ffffff;
  align-items: center;
  justify-content: center;
`;

const App = () => {
  return (
    <Container>
      <Button title="Hanbit" />
      <Button title="React Native" />
    </Container>
  );
};

export default App;
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/simulator_screen3.png?raw=true"
  style="width: 50%;"
/>

스타일드 컴포넌트를 사용하면 이렇게 역할에 맞는 이름을 지정할 수 있다는 장점이 있어요.

확실히 앞으로는 리액트 네이티브에서 제공하는 스타일시트를 사용하지 않고 스타일드 컴포넌트를 사용하는게 좋겠어요.

#### props 사용하기

스타일드 컴포넌트에서 스타일을 작성하는 백틱 안에서 props에 접근할 수 있다는 장점을 이용해 스타일을 작성하는 곳에서 조건에 따라 스타일을 변경할 수 있어요.

```jsx
...
const ButtonContainer = styled.TouchableOpacity`
  background-color: ${(props) =>
    props.title === "Hanbit" ? "#3498db" : "#9b59b6"};
  border-radius: 15px;
  padding: 15px 40px;
  margin: 10px 0px;
  justify-content: center;
`;
const Title = styled.Text`
  font-size: 20px;
  font-weight: 600;
  color: #fff;
`;

const Button = (props) => {
  return (
    <ButtonContainer title={props.title}>
      <Title>{props.title}</Title>
    </ButtonContainer>
  );
};
...
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/simulator_screen4.png?raw=true"
  style="width: 50%;"
/>

이런식으로 ButtonContainer 컴포넌트에 props로 title을 전달하여 배경색을 설정하는 곳에서 title의 값에 따라 다른 색이 지정되도록 할 수 있어요.

#### attrs 사용하기

attrs를 이용하면 스타일을 설정하는 곳에서 props의 값에 따라 컴포넌트의 속성을 다르게 적용할 수도 있고 항상 일정한 속성을 미리 정의해놓을 수 있어요.

```jsx
// src/components/Input.js
import React from "react";
import styled from "styled-components/native";

const StyledInput = styled.TextInput.attrs((props) => ({
  placeholder: "Enter a text...",
  placeholderTextColor: props.borderColor,
}))`
  width: 200px;
  height: 60px;
  margin: 5px;
  padding: 10px;
  border-radius: 10px;
  border: 2px;
  border-color: ${(props) => props.borderColor};
  font-size: 24px;
`;

const Input = (props) => {
  return <StyledInput borderColor={props.borderColor} />;
};

export default Input;
```
```jsx
// src/App.js
import React from "react";
import styled from "styled-components/native";
import Button from "./components/Button";
import Input from "./components/Input";

const Container = styled.View`
  flex: 1;
  background-color: #ffffff;
  align-items: center;
  justify-content: center;
`;
const App = () => {
  return (
    <Container>
      <Button title="Hanbit" />
      <Button title="React Native" />
      <Input borderColor="#3498db" />
      <Input borderColor="#9b59b6" />
    </Container>
  );
};
export default App;
```

#### ThemeProvider

드디어 마지막이네요.

**ThemeProvider**는 Context API를 활용해 애플리케이션 전체에서 스타일드 컴포넌트를 이용할 때 미리 정의한 값들을 사용할 수 있도록 props로 전달해요.

ThemeProvider를 이용해서 다크모드와 라이트모드를 전환하는 기능을 실습해보았어요.

```jsx
// src/App.js
import React, { useState } from "react";
import styled, { ThemeProvider } from "styled-components/native";
import Button from "./components/Button";
import Input from "./components/Input";
import { lightTheme, darkTheme } from "./theme";
import { Switch } from "react-native";

const Container = styled.View`
  flex: 1;
  background-color: ${(props) => props.theme.background};
  align-items: center;
  justify-content: center;
`;
const App = () => {
  const [isDark, setIsDark] = useState(false);
  const _toggleSwitch = () => setIsDark(!isDark);

  return (
    <ThemeProvider theme={isDark ? darkTheme : lightTheme}>
      <Container>
        <Switch value={isDark} onValueChange={_toggleSwitch} />
        <Button title="Hanbit" />
        <Button title="React Native" />
        <Input borderColor="#3498db" />
        <Input borderColor="#9b59b6" />
      </Container>
    </ThemeProvider>
  );
};
export default App;
```
```jsx
// src/components/Button.js
import React from "react";
import styled from "styled-components/native";

const ButtonContainer = styled.TouchableOpacity`
  background-color: ${(props) =>
    props.title === "Hanbit" ? props.theme.blue : props.theme.purple};
  border-radius: 15px;
  padding: 15px 40px;
  margin: 10px 0px;
  justify-content: center;
`;
const Title = styled.Text`
  font-size: 20px;
  font-weight: 600;
  color: ${(props) => props.theme.text};
`;

const Button = (props) => {
  return (
    <ButtonContainer title={props.title}>
      <Title>{props.title}</Title>
    </ButtonContainer>
  );
};

export default Button;
```
```jsx
// src/theme.js
export const lightTheme = {
  background: "#ffffff",
  text: "#ffffff",
  purple: "#9b59b6",
  blue: "#3498db",
};

export const darkTheme = {
  background: "#34495e",
  text: "#34495e",
  purple: "#9b59b6",
  blue: "#3498db",
};
```

이렇게 세 파일로 구성했고, 라이트 모드와 다크 모드일 때 변경시킬 스타일들에 전부 props로 상태에 맞게 변화하도록 구현했어요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/simulator_screen5.png?raw=true"
  style="width: 50%;"
/>

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter4/simulator_screen6.png?raw=true"
  style="width: 50%;"
/>

이런식으로 변경이 돼요!

왜인지 모르겠지만 저의 Switch는 가운데로 오지 않네요..

현재 프로젝트를 RN을 사용하고 다크 모드 구현을 생각을 했었는데 이렇게 빨리 다크모드를 사용하는 방법을 알게되었어요.

