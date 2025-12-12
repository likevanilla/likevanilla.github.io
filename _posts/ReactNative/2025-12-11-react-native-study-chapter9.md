---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 7-1"
description: "[처음부터 배우는 리액트 네이티브] Chapter 9 채팅 어플리케이션"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-12-11
last_modified_at: 2025-12-11
---

* 목차
{:toc}

[처음 배우는 리액트 네이티브] 책으로 리액트 네이티브 공부하기!

## 들어가기 전...
이전 글에서 발생했던 문제인 Github 블로그에서 
{%raw%}
```markdown
{{}}
```
{%endraw%}
가 글 내용에 포함되면, Liquid 오류가 발생해서 push가 되지 않던 문제!!

이유는 Liquid 템플릿 문법과 충돌했기 때문인데..

Liquid에서 
{%raw%}
```markdown
{{ 변수 }}
```
{%endraw%} 
이런식으로 변수 출력을 하기 때문에 글을 쓸 때 중괄호 2개를 써버리면 변수로 인식된다~

그래서 해결 방법은 코드 블럭을 감싸게 {% raw %}, {% endraw %}를 위 아래로 추가해주면 문제가 해결이 된다!!!

## 채팅 애플리케이션

이번에는 채팅 애플리케이션을 만들어볼 거에요.

**기능**
- 로그인/회원가입
- 프로필
- 채널 생성
- 채널 목록
- 채널

두 번으로 나눠서 진행을 할게요!

### 파이어베이스

파이어베이스는 인증, 데이터베이스 등의 다양한 기능을 제공하는 개발 플랫폼이에요.

파이어베이스가 제공하는 기능을 이용하면 대부분의 서비스에서 필요한 서버와 데이터베이스를 직접 구축하지 않아도 개발이 가능해요.

이번 프로젝트에서 파이어베이스를 활용해볼 거에요.

기본적인 파이어베이스 설정은 [이 블로그](https://velog.io/@choi-ss-choco/처음-배우는-리액트-네이티브-채팅-어플리케이션-만들기-1.-파이어베이스-설정)를 참고했어요!

### 앱 아이콘과 로딩 화면

우선 교재에서 사용한 expo 패키지의 AppLoading은 이제 없다 ! 그래서 오류가 발생했는데...

해결 방법은 새로운 패키지를 설치해서 바꿔주었다.

```bash
expo install expo-app-loading
```

그러고나서 import 부분도 수정을 해줘야 한다.

그런데도 expo-app-loading이 deprecated이기 때문에 실행은 되지만 오류가 뜬다..

스플래시 이미지가 뜨지 않지만 일단은 실행이 되니 넘어가고 나중에 수정을 해줘야겠다.

참고로 GPT가 expo-splash-screen을 추천해준다.

궁금해서 검색해보니 2020에 시작해서 2025-12-05에 최신 업데이트가 있었다.

[expo-splash-screen 공식문서](https://docs.expo.dev/versions/latest/sdk/splash-screen/)

그래서 완성된 코드는 아래와 같다.

{% raw %}
```jsx
// src/App.js
import React, { useState } from "react";
import { StatusBar, Image } from "react-native";
import AppLoading from "expo-app-loading";
import { Asset } from "expo-asset";
import * as Font from "expo-font";
import { ThemeProvider } from "styled-components/native";
import { theme } from "./theme";

const cacheImages = (images) => {
  return images.map((image) => {
    if (typeof image === "string") {
      return Image.prefetch(image);
    } else {
      return Asset.fromModule(image).downloadAsync();
    }
  });
};

const cacheFonts = (fonts) => {
  return fonts.map((font) => Font.loadAsync(font));
};

const App = () => {
  const [isReady, setIsReady] = useState(false);

  const _loadAssets = async () => {
    const imageAssets = cacheImages([require("../assets/splash.png")]);
    const fontAssets = cacheFonts([]);

    await Promise.all([...imageAssets, ...fontAssets]);
  };

  return isReady ? (
    <ThemeProvider theme={theme}>
      <StatusBar barStyle="dark-content" />
    </ThemeProvider>
  ) : (
    <AppLoading
      startAsync={_loadAssets}
      onFinish={() => setIsReady(true)}
      onError={console.warn}
    />
  );
};

export default App;

```
{% endraw %}

코드를 분석하자면, 앞으로 프로젝트에서 사용할 이미지와 폰트를 미리 불러와서 사용할 수 있도록 cacheImages와 cacheFonts 함수를 작성하고 이를 이용해 _loadAssets 함수를 구성했어요.

이렇게 구성하면 이미지나 폰트가 느리게 적용되는 문제를 해결할 수 있어요.

애플리케이션은 미리 불러와야 하는 항목들을 모두 불러오고 화면이 렌더링되도록 AppLoading 컴포넌트의 startAsync에 _loadAssets 함수를 지정하고, 완료되었을 때 isReady 상태를 변경해서 화면이 렌더링되도록 작성했어요.

### 인증 화면

파이어베이스의 인증 기능을 이용해서 로그인 화면과 회원가입 화면을 만들어 보아요.

#### 내비게이션

우선 로그인 화면과 회원가입 화면을 만들어요.

{% raw %}
```jsx
// src/screens/Login.js
import React from "react";
import styled from "styled-components/native";
import { Text, Button } from "react-native";

const Container = styled.View`
  flex: 1;
  justify-content: center;
  align-items: center;
  background-color: ${({ theme }) => theme.background};
`;

const Login = ({ navigation }) => {
  return (
    <Container>
      <Text style={{ fontSize: 30 }}>Login Screen</Text>
      <Button title="Signup" onPress={() => navigation.navigate("Signup")} />
    </Container>
  );
};

export default Login;
```
{% endraw %}

{% raw %}
```jsx
// src/screens/Signup.js
import React from "react";
import styled from "styled-components/native";
import { Text } from "react-native";

const Container = styled.View`
  flex: 1;
  justify-content: center;
  align-items: center;
  background-color: ${({ theme }) => theme.background};
`;

const Signup = () => {
  return (
    <Container>
      <Text style={{ fontSize: 30 }}>Signup Screen</Text>
    </Container>
  );
};

export default Signup;
```
{% endraw %}

```jsx
// src/screens/index.js
import Login from "./Login";
import Signup from "./Signup";

export { Login, Signup };
```

아래는 스택 네비게이션을 이용하는 코드에요.

{% raw %}
```jsx
// src/navigations/AuthStack.js
import React, { useContext } from "react";
import { ThemeContext } from "styled-components/native";
import { createStackNavigator } from "@react-navigation/stack";
import { Login, Signup } from "../screens";

const Stack = createStackNavigator();

const AuthStack = () => {
  const theme = useContext(ThemeContext);
  return (
    <Stack.Navigator
      initialRouteName="Login"
      screenOptions={{
        headerTitleAlign: "center",
        cardStyle: { backgroundColor: theme.background },
      }}
    >
      <Stack.Screen name="Login" component={Login} />
      <Stack.Screen name="Signup" component={Signup} />
    </Stack.Navigator>
  );
};

export default AuthStack;
```
{% endraw %}

헤더의 타이틀 위치를 안드로이드와 iOS에서 동일한 위치에 렌더링하기 위해 headerTitleAlign의 값을 center로 설정했어요.

```jsx
// src/navigations/index.js
import React from "react";
import { NavigationContainer } from "@react-navigation/native";
import AuthStack from "./AuthStack";

const Navigation = () => {
  return (
    <NavigationContainer>
      <AuthStack />
    </NavigationContainer>
  );
};

export default Navigation;
```

이제 App 컴포넌트에 추가를 해주면 네비게이션이 잘 작동할 거에요.

```jsx
// src/App.ks
...
import Navigation from "./navigations";
...
const App = () => {
  ...
  return isReady ? (
    <ThemeProvider theme={theme}>
      <StatusBar barStyle="dark-content" />
      <Navigation />
      ...
  )
}
...
```

<div style="display: flex; gap: 10px;">
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter9/simulator1.png?raw=true"
    style="width: 50%;"
  />
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter9/simulator2.png?raw=true"
    style="width: 50%;"
  />
</div>

#### 로그인 화면

Image 컴포넌트와 로고 적용하기 부분은 문제가 있었어요.

로고를 적용하려면 파이어베이스에서 storage를 사용해야되는데, storage를 사용하려면 유료로 프로젝트를 이용해야함!

그래서 일단은 큰 지장이 없을 것 같아서 스킵하고 진행하기로 했어요.

앞으로는 코드가 너무 많이 나와서 코드는 적지 않고 코드에 대한 설명들만 이어가고, 중요하다고 생각되는 코드들만 따로 짚어볼게요.

##### Input 컴포넌트

//src/components/Input,js 파일에 작성한 코드에서 Input 컴포넌트에 secureTextEntry 속성을 사용했어요.

`secureTextEntry` 속성은 입력되는 문자를 감추는 기능으로 비밀번호를 입력하는 곳에서 많이 사용돼요.

이메일을 입력 받고 returnKeyType 속성의 next를 이용해서 비밀번호 Input 컴포넌트로 포커스를 바꾸기 위해서 useRef를 이용했어요.

useRef를 이용해서 passwordRef를 만들고 비밀번호를 입력하는 Input 컴포넌트의 ref로 지정했어요.

이메일 Input 컴포넌트의 onSubmitEditing 함수를 passwordRef를 이용해서 비밀번호 Input 컴포넌트로 포커스가 이동되도록 했어요.

이제 Input 컴포넌트에 전달된 ref를 이용해서 TextInput 컴포넌트의 ref로 지정해야 하는데, ref는 key처럼 리액트에서 특별히 관리되기 때문에 자식 컴포넌트의 props로 전달되지 않아요.

이러한 상황에서 `forwardRef` 함수를 이용하면 ref를 전달받을 수 있어요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter9/simulator3.png?raw=true"
  style="width: 50%;"
/>

#### 키보드 감추기

키보드 감추기는 `react-native-keyboard-aware-scroll-view` 라이브러리를 사용해요.

이 라이브러리는 포커스가 있는 TextInput 컴포넌트의 위치로 자동 스크롤되는 기능 등 Input 컴포넌트에 필요한 기능들을 제공해요.

입력 도중 다른 영역을 터치했을 때 키보드가 사라질 뿐만 아니라 포커스를 얻은 TextInput 컴포넌트의 위치에 맞춰 스크롤이 이동하고, 스크롤되는 위치를 조정하고 싶은 경우 **extraScrollHeight**의 값을 조절해서 원하는 위치로 스크롤되도록 설정할 수 있어요.

#### 오류 메세지

Input 컴포넌트에 입력되는 값이 올바른 형태로 입력되었는지 확인하고, 잘못된 값이 입력되면 오류 메세지를 보여주는 기능을 구현했아요.

utils 폴더에 common.js 파일을 생성하고 올바른 이메일 형식인지 확인하는 함수와 입력된 문자열에서 공백을 모두 제거하는 함수를 만들었어요.

```jsx
const regex =
    /^[0-9?A-z0-9?]+(\.)?[0-9?A-z0-9?]+@[0-9?A-z]+\.[A-z]{2}.?[A-z]{0,3}$/;
```

이런식으로 코드를 구성해서 유효한 이메일 형식인지 확인을 해요.

- `^` : 문자열 시작
- `$` : 문자열 끝
- `[]` : 문자 클래스
- `?` : 물음표 문자 자체(대괄호 내부), 있어도 되고 없어도 됨((\.)**?**)
- `+` : 1글자 이상
- `0-9` : 숫자
- `A-z` : 대문자 A ~ 소문자 z
- `\.` : 진짜 점
- `()` : 그룹
- `@` : 그냥 이메일 구분자

이런식으로 코드의 구조를 이해할 수 있을 것 같아요!

#### Button 컴포넌트, 헤더 수정하기, 노치 디자인 대응

특별히 중요하거나 처음 보는 기능이 있지는 않기도 하고, 버튼이 변하지가 않아서 넘어갈게요..

### 회원가입 화면

회원가입 화면을 만드는 것은 로그인 화면을 구현하는 것과 크게 다르지 않았어요.

#### 사진 입력받기

이 부분도 파이어베이스의 Storage 관련 문제가 해결되지 않아서 지나가려 했지만, 그저 이미지를 가져오는 부분이 아니라서 주의깊게 봤어요.

회원가입을 할 때 사용자에게 사진을 입력받는 기능을 구현할 거에요.

우선 사진이 입력되지 않았을 때 사용될 기본 이미지를 추가해요.

그리고 `expo-image-picker` 라이브러리를 이용해서 갤러리 접근 기능을 구현해요.

**iOS**에서는 사진첩에 접근하지 위해 사용자에게 권한을 요청하는 과정이 필요하므로 권한을 요청하는 부분이 추가되고, **안드로이드**는 특별한 설정 없이 사진에 접근할 수 있기 때문에 따로 추가를 하지 않았어요.

Expo 프로젝트에서 안드로이드는 기본적으로 모든 권한을 포함하고 있어 특별한 설정이 필요 없어요.

권한을 요청할 때 나타나는 창의 메세지는 `app.json` 파일을 수정해서 변경할 수 있어요.

사진에 접근하기 위해 호출되는 라이브러리 함수는 다음과 같은 값들을 포함한 객체를 파라미터로 전달 받아요.

- mediaTypes: 조회하는 자료의 타입
- allowsEditing: 이미지 선택 후 편집 단계 진행 여부
- aspect: 안드로이드 전용 옵션으로 이미지 편집 시 사각형의 비율([x, y])
- quality: 0~1 사이의 값을 받으며 압축 품질을 의미(1 = 최대 품질)

### 로그인과 회원가입

#### 로그인 

`signInWithEmailAndPassword` 함수는 이메일과 비밀번호를 이용해서 인증받는 함수에요.

#### 회원가입

`createUserWithEmailAndPassword` 함수는 이메일과 비밀번호를 이용해서 사용자를 생성하는 함수에요.

회원가입 기능을 이용해서 사용자를 추가하면 파이어베이스 콘솔에서 추가된 사용자를 확인할 수 있어요.

이렇게 추가된 사용자는 `uid`라는 사용자마다 갖고 있는 유일한 키 값으로 식별해요.

이후 파이어베이스 스토리지의 보안 규칙을 마음대로 수정할 수 있는데, 여기서 시뮬레이션 유형을 변경하면서 다양한 상황을 만들 수 있고, 임의의 uid를 지정해서 인증된 사용자의 접근 여부를 테스트할 수 있다고 해요.

#### Spinner 컴포넌트

여기서 사용된 Spinner 컴포넌트는 로그인 혹은 회원가입이 진행되는 동안에 데이터를 수정하거나 버튼을 추가로 클릭하는 일이 발생하지 않도록 예방하는 용도로 사용돼요.

Spinner 컴포넌트가 화면 전체를 차지하게 하면서 사용자가 다른 행동을 취할 수 없도록 다른 컴포넌트보다 위에 있게 작성을 해서 구현해요.

Spinner 컴포넌트를 AuthStack 내비게이션의 하위 컴포넌트로 사용하면 내비게이션을 포함한 화면 전체를 차지할 수 없기 때문에 navigations 폴더의 index.js에서 AuthStack 내비게이션과 같은 위치에 Spinner 컴포넌트를 사용해요.

## 후기
> 지금 챕터 9의 반 정도만 한 건데 버전도 잘 안 맞고 그래서 실습이 제대로 되지 않고 있어요.. 점점 코드는 늘어날텐데 이 뒤는 어떡하죠??!!
> {:.lead}