---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 6"
description: "[처음부터 배우는 리액트 네이티브] Chapter 8 내비게이션"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-11-28
last_modified_at: 2025-11-28
---

* 목차
{:toc}

[처음 배우는 리액트 네이티브] 책으로 리액트 네이티브 공부하기!

### 리액트 내비게이션

모바일 앱은 하나의 화면으로 구성되는 경우가 거의 없기 때문에 내비게이션이 가장 중요한 기능 중 하나라고 할 수 있어요.

리액트 네이티브에서는 내비게이션 기능을 지원하지 않으므로 의부 라이브러리를 이용해야 해요.

리액트 내비게이션 라이브러리가 지원하는 내비게이션 종류는 스택, 탭, 드로어로 세 종류가 있어요.

현재 최신 버전은 7.x 버전이라 교재에서 다루는 5.x 버전과는 다른 점들이 있으니 공식 페이지를 확인하는 것이 좋을 것 같아요

[리액트 내비게이션 라이브러리](https://reactnavigation.org)

#### 내비게이션 구조

리액트 내비게이션에는 **NavigationContainer 컴포넌트**, **Navigator 컴포넌트**, **Screen 컴포넌트**가 있어요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/image1.jpeg?raw=true"
  style="width: 50%;"
/>

**Screen 컴포넌트**는 화면으로 사용되는 컴포넌트로 name과 component 속성을 지정해야 해요.
name은 화면 이름으로 사용되고 component에는 화면으로 사용될 컴포넌트를 전달해요.
화면으로 사용되는 컴포넌트에는 항상 navigation과 route가 props로 전달된다는 특징이 있어요.

**Navigator 컴포넌트**는 화면을 관리하는 중간 관리자 역할로 내비게이션을 구성하며, 여러 개의 Screen 컴포넌트를 자식 컴포넌트로 갖고 있어요.

**NavigationContainer 컴포넌트**는 내비게이션의 계층 구조와 상태를 관리하는 컨터에니 역할을 하며, 모든 내비게이션 구성 요소를 감싼 최상위 컴포넌트여야 해요.

#### 설정 우선 순위

화면으로 사용되는 컴포넌트에서 설정하는 방법과 Screen 컴포넌트에서 설정하는 방법은 **해당 화면에만 적용**되지만, Navigator 컴포넌트에서 속성을 저장하는 방법을 사용하면 **자식 컴포넌트로 존재하는 모든 컴포넌트에 적용**된다는 특징이 있어요.

**모든 화면에 공통적으로 적용하고 싶은 속성인 경우 Navigator 컴포넌트**를 이용하고, **개별 화면에만 적용되는 속성인 경우 Screen 컴포넌트** 혹은 확면으로 사용되는 컴포넌트에서 설정하는 것이 좋아요.

**작은 범위의 설정일수록 우선순위가 높으므로** 만약 Screen 컴포넌트와 화면으로 사용되는 컴포넌트에 동일한 옵션이 적용되었다면 화면 컴포넌트의 설정을 우선시 해요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/image2.png?raw=true"
  style="width: 50%;"
/>

#### 라이브러리 설치

리액트 내비게이션은 각 기능별로 모듈이 분리되어 있어, 이후에도 사용하는 내비게이션의 종류에 따라 개별적으로 추가 라이브러리를 설치해야 해요.

```bash
npm install --save @react-navigation/native

expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

교재를 따라 이렇게 설치를 했어요.

expo에서는 --force가 먹히지 않아서 일단 그냥 진행을 했어요! 제대로 다 설치가 된 느낌은 아니에요..

### 스택 내비게이션

가장 기본적인 스택 내비게이션을 먼저 해볼게요

```bash
npm i @react-navigation/stack
```

저는 역시 --force 옵션을 붙여서 설치를 했어요.

스택 내비게이션의 특징은 현재 화면 위에 다른 화면을 쌓으면서 화면을 이동하는 것이 특징이에요.

화면 위에 새로운 화면을 쌓으면서(push) 이동하기 때문에 이전 화면을 계속 유지해요.

이런 구조 때문에 가장 위에 있는 화면을 들어내면(pop) 이전 화면으로 돌아갈 수 있다는 특징이 있어요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/image3.jpeg?raw=true"
  style="width: 50%;"
/>

#### 화면 구성

```jsx
// src/screens.Home.js
import React from "react";
import { Button } from "react-native";
import styled from "styled-components";

const Container = styled.View`
  align-items: center;
`;

const StyledText = styled.Text`
  font-size: 30px;
  margin-bottom: 10px;
`;

const Home = () => {
  return (
    <Container>
      <StyledText>Home</StyledText>
      <Button title="go to the list screen" />
    </Container>
  );
};

export default Home;
```

```jsx
// scr/screens/List.js

import React from "react";
import { Button } from "react-native";
import styled from "styled-components";

const Container = styled.View`
  flex: 1;
  justify-content: center;
  align-items: center;
`;

const StyledText = styled.Text`
  font-size: 30px;
  margin-bottom: 10px;
`;

const items = [
  { _id: 1, name: "React Native" },
  { _id: 2, name: "React Navigation" },
  { _id: 3, name: "정혁" },
];

const List = () => {
  const _onPress = (item) => {};

  return (
    <Container>
      <StyledText>List</StyledText>
      {items.map((item) => (
        <Button
          key={item._id}
          title={item.name}
          onPress={() => _onPress(item)}
        />
      ))}
    </Container>
  );
};

export default List;
```

```jsx
// src/screens/Item.js
import React from "react";
import styled from "styled-components";

const Container = styled.View`
  flex: 1;
  justify-content: center;
  align-items: center;
`;

const StyledText = styled.Text`
  font-size: 30px;
  margin-bottom: 10px;
`;

const Item = () => {
  return (
    <Container>
      <StyledText>Item</StyledText>
    </Container>
  );
};

export default Item;
```

```jsx
// src/navigations/Stack.js

import React from "react";
import { createStackNavigator } from "@react-navigation/stack";
import Home from "../screens/Home";
import List from "../screens/List";
import Item from "../screens/Item";

const Stack = createStackNavigator();

const StackNavigator = () => {
  return (
    <Stack.Navigator>
      <Stack.Screen name="Home" component={Home} />
      <Stack.Screen name="List" component={List} />
      <Stack.Screen name="Item" component={Item} />
    </Stack.Navigator>
  );
};
export default StackNavigator;
```

Home, List, Item 컴포넌트를 만들고 **createStackNavigator 함수**를 이용해서 스택 내비게이션을 생성했어요.

생성된 스택 내비게이션에는 화면을 구성하는 Screen 컴포넌트와 Screen 컴포넌트를 관리하는 Navigator 컴포넌트가 있어요.

Screen 컴포넌트의 **name은 반드시 서로 다른 값**을 가져야 해요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator1.png?raw=true"
  style="width: 50%;"
/>

**Navigator 컴포넌트의 첫 번째 자식** Screen 컴포넌트가 첫 번째 화면으로 나와요.

그래서 Home 첫 화면으로 나올텐데, 여기서 List를 첫 번째 자식으로 두거나 **initialRouteName 속성**을 이용해서 첫 화면으로 설정할 수 있어요.

```jsx
// src/navigations/Stack.js

const StackNavigator = () => {
  return (
    <Stack.Navigator>
      <Stack.Screen name="List" component={List} />
      <Stack.Screen name="Home" component={Home} />
      <Stack.Screen name="Item" component={Item} />
    </Stack.Navigator>
  );
};

// 또는

const StackNavigator = () => {
  return (
    <Stack.Navigator initialRouteName="List">
      <Stack.Screen name="List" component={List} />
      <Stack.Screen name="Home" component={Home} />
      <Stack.Screen name="Item" component={Item} />
    </Stack.Navigator>
  );
};
```

#### 화면 이동

Screen 컴포넌트의 component로 지정된 컴포넌트는 화면으로 이동되고 navigation이 props로 전달돼요.

**navigate 함수**는 원하는 화면으로 이동하는 데 사용되는 함수에요.

```jsx
// src/screens/Home.js
...
const Home = ({ navigation }) => {
  return (
    <Container>
      <StyledText>Home</StyledText>
      <Button
        title="go to the list screen"
        onPress={() => navigation.navigate("List")}
      />
    </Container>
  );
};
...
```

navigation에 있는 navigate 함수를 이용해서 원하는 화면의 이름을 전달하면 해당 화면으로 이동해요.

전달되는 화면의 이름은 Screen 컴포넌트의 name값 중 하나를 입력해야 해요.

이제 Home에서 "go to the list screen"을 누르면 List 화면으로 이동해요.

navigate 함수를 이용할 때 **두 번째 파라미터에 객체를 전달해서 이동하는 화면에 필요한 정보를 함께 전달하는 기능**이 있어요.

```jsx
// src/screens/List.js
...
const List = ({ navigation }) => {
  const _onPress = (item) => {
    navigation.navigate("Item", { id: item._id, name: item.name });
  };
  ...
};
...
```

Item 화면으로 이동하면서 항목의 id와 name을 함께 전달하도록 했어요.

```jsx
// src/screens/Item.js
...
const Item = ({ route }) => {
  return (
    <Container>
      <StyledText>Item</StyledText>
      <StyledText>ID: {route.params.id}</StyledText>
      <StyledText>Name: {route.params.name}</StyledText>
    </Container>
  );
};
...
```

Item 화면에서 전달되는 params를 이용하여 화면에 항목의 id와 name을 출력하게 했어요.

route를 통해 데이터를 전달해요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator2.png?raw=true"
  style="width: 50%;"
/>

#### 화면 배경색 수정하기

Home 화면의 배경색을 흰색으로 설정하면 다음과 같은 모습이 나와요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator3.png?raw=true"
  style="width: 50%;"
/>

내비게이션은 화면 전체를 차지하고 있지만 화면으로 사용된 컴포넌트의 영역이 전체를 차지하고 있지 않아서 나타나는 문제에요.

스타일에 flex: 1을 설정하는 방법과 리액트 내비게이션의 설정을 수정하면 해결할 수 있어요.

```jsx
// src/navigations/Stack.js
...
    <Stack.Navigator
      initialRouteName="Home"
      screenOptions={{ cardStyle: { backgroundColor: "#ffffff" } }}
    >
...
```

**cardStyle**을 이용하면 스택 내비게이션의 화면 배경색을 수정할 수 있어요.

Navigator 컴포넌트의 **screenOptions**에 설정을 하면 화면 전체에 적용시킬 수 있어요.

#### 헤더 수정하기

스택 내비게이션의 **헤더**는 뒤로 가기 버튼을 제공하거나 타이틀을 통해 현재 화면을 알려주는 역할을 해요.

##### 타이틀 수정하기

헤더의 타이틀은 **Screen 컴포넌트의 name 속성을 기본값**으로 사용해요.

그래서 타이틀을 변경하는 가장 쉬운 방법은 **name을 원하는 값으로 수정**하는 거에요.

하지만 name 속성을 이용하는 곳을 찾아다니며 모두 수정해야 한다는 단점이 있어요.

```jsx
// src/navigations/Stack.js
...
  <Stack.Screen
    name="List"
    component={List}
    options={{ headerTitle: "List Screen" }}
  />
...
```

이렇게 개별 화면 설정을 수정하고 싶은 경우 Screen 컴포넌트의 **options**를 이용하고, 화면 타이틀을 변경하기 위해 **headerTitle 속성**을 이용했어요.

모든 화면에서 같은 타이틀이 나타나도록 수정할 때는 Navigator 컴포넌트의 **screenOptions 속성**에 headerTitle을 저장하면 돼요.

#### 스타일 수정하기

헤더의 배경색 등을 수정하는 **headerStyle**

헤더의 타이틀 컴포넌트의 스타일을 수정하는 **headerTitleStyle**

```jsx
// src/navigations/Stack.js
...
    <Stack.Navigator
      initialRouteName="Home"
      screenOptions={{
        cardStyle: { backgroundColor: "#ffffff" },
        headerStyle: {
          height: 110,
          backgroundColor: "#95a5a6",
          borderBottomWidth: 5,
          borderBottomColor: "#34495e",
        },
        headerTitleStyle: { color: "#ffffff", fontSize: 24 },
      }}
    >
...
```

헤더에 스타일을 적용 해보았어요.

하지만 안드로이드와 iOS에서 차이점이 있어요.

iOS는 타이틀이 중앙으로 정렬되어 있지만 안드로이드는 그렇지 않아요.

<div style="display: flex; gap: 10px;">
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator4.png?raw=true"
    style="width: 50%;"
  />
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator5.png?raw=true"
    style="width: 50%;"
  />
</div>

그래서 둘 다 동일하게 하기 위해서 headerTitleAlign 속성을 이용해요.

**headerTitleAlign 속성**에는 left와 center 두 가지 값 중 하나만 설정할 수 있는데 iOS는 center, 안드로이드는 left가 기본값으로 설정되어 있어요.

그래서 `headerTitleAlign: 'center'` 코드를 추가해서 통일을 시켜요.

#### 타이틀 컴포넌트 변경

헤더의 타이틀을 변경하기 위해 문자열을 지정했던 headerTitle 속성에 컴포넌트를 반환하는 함수를 지정하면 타이틀 컴포넌트를 반환하는 컴포넌트로 변경할 수 있어요.

headerTitle에 함수가 설정되면 해당 함수의 파라미터로 style과 tintColor 등이 포함된 객체가 전달돼요.

그중 style은 headerTitleStyle에 설정된 값이고, tintColor는 headerTintColor에 지정된 값이 전달돼요.

```jsx
// src/navigations/Stack.js
...
import { MaterialCommunityIcons } from "@expo/vector-icons";
...
    <Stack.Navigator
      initialRouteName="Home"
      screenOptions={{
        cardStyle: { backgroundColor: "#ffffff" },
        headerStyle: {
          height: 110,
          backgroundColor: "#95a5a6",
          borderBottomWidth: 5,
          borderBottomColor: "#34495e",
        },
        headerTitleStyle: { color: "#ffffff", fontSize: 24 },
        headerTitleAlign: "center",
        headerTitle: ({ style }) => (
          <MaterialCommunityIcons name="react" style={style} />
        ),
      }}
    >
...
```

vector-ions는 Expo 프로젝트에서 기본적으로 설치되는 라이브러리에요.

#### 버튼 수정하기

스택 내비게이션에서 화면을 이동하면 헤더 왼쪽에 이전 화면으로 이동하는 뒤로 가기 버튼이 나타나요.

만약 첫 화면처럼 이전 화면이 없는 경우에는 버튼이 생기지 않아요.

뒤로 가기 버튼을 이용하는 방법 외에 화면 왼쪽 끝에서 오른쪽 방향으로 **스와이프 제스쳐**를 통해 이전 화면으로 돌아가는 방법도 있어요.

안드로이드는 버튼의 타이틀을 보여주지 않고, iOS는 이전 화면의 타이틀을 버튼의 타이틀로 보여주는 차이점이 있어요.

~~**headerBackTitleVisible**을 이용하면 두 플랫폼의 버튼 타이틀 렌더링 여부를 동일하게 할 수 있어요.~~

버전 6부터 headerBackTitleVisible이 아닌 **headerBackButtonDisplayMode**로 변경되었으며 "default", "generic", "minimal" 세 가지 모드가 있어요.

- **default 모드**에서는 화살표와 이전 화면의 Title이 표시돼요.

- **generic 모드**에서는 화살표와 back이 표시돼요.

- **minimal 모드**에서는 화살표만 표시돼요.

이전 화면의 이름이 아닌 다른 값을 사용하고 싶은 경우 **headerBackTitle**을 이용해요.

```jsx
// src/navigations/Stack.js
...
<Stack.Screen
  name="List"
  component={List}
  options={{
    headerTitle: "List Screen",
    headerBackButtonDisplayMode: "default",
    headerBackTitle: "Prev",
  }}
/>
...
```

iOS와 안드로이드 둘 다 HOME으로 가는 버튼은 Prev로 잘 나타났지만 Item으로 들어갔을 때 iOS는 List Screen, 안드로이드는 그냥 버튼만 있었어요.

그리고 버튼 타이틀을 수정하려면 버튼 디스플레이 모드를 default로 설정을 해줘야 해요.

#### 버튼 스타일 수정하기

**headerBackTitleStyle**은 글자의 색뿐만 아니라 글자 크기 등 다양한 스타일을 지정할 수 있지만 버튼의 타이틀에만 적용돼요.

버튼의 타이틀과 이미지의 색을 동일하게 변경하려면 **headerTintColor**를 이용해야 해요.

headerTintColor에 지정된 색은 버튼뿐만 아니라 헤더의 타이틀에도 적용되지만, headerTintStyle 혹은 headerBackTitleStyle이 우선순위가 높으므로 headerTintColor에 설정한 색으로 나타나게 하고 싶다면 다른 스타일과 겹치지 않도록 작성하는 것이 중요해요.

```jsx
// src/navigations/Stack.js
...
<Stack.Screen
  name="List"
  component={List}
  options={{
    headerTitle: "List Screen",
    headerBackButtonDisplayMode: "default",
    headerBackTitle: "Prev",
    headerTitleStyle: { fontSize: 24 },
    headerTintColor: "#e74c3c",
  }}
/>
...
```

#### 버튼 컴포넌트 변경

**headerBackImage**에 컴포넌트를 반환하는 함수를 전달해서 두 플랫폼이 동일한 이미지를 렌더링하도록 변경할 수 있어요.

<div style="display: flex; gap: 10px;">
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator6.png?raw=true"
    style="width: 50%;"
  />
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator7.png?raw=true"
    style="width: 50%;"
  />
</div>

버튼을 동일하게 변경한 부분까지의 화면이에요.

뒤로 가기 버튼의 이미지가 아니라 헤더의 왼쪽 버튼 전체를 변경하고 싶다면 headerLeft에 컴포넌트를 반환하는 함수를 지정해요.

이와 동일한 방법으로 headerRight에 컴포넌트를 반환하는 함수를 지정하면 헤더의 오른쪽에 원하는 컴포넌트를 렌더링할 수 있어요.

```jsx
// src/screens.Item.js

import React, { useLayoutEffect } from "react";
import styled from "styled-components";
import { MaterialCommunityIcons } from "@expo/vector-icons";

...

const Item = ({ navigation, route }) => {
  useLayoutEffect(() => {
    navigation.setOptions({
      headerBackButtonDisplayMode: "default",
      headerTintColor: "#ffffff",
      headerLeft: ({ onPress, tintColor }) => (
        <MaterialCommunityIcons
          name="keyboard-backspace"
          size={30}
          style={{ marginLeft: 11 }}
          color={tintColor}
          onPress={onPress}
        />
      ),
      headerRight: ({ tintColor }) => (
        <MaterialCommunityIcons
          name="home-variant"
          size={30}
          style={{ marginRight: 11 }}
          color={tintColor}
          onPress={() => navigation.popToTop()}
        />
      ),
    });
  }, []);
  return (
      ...
  );
};
...
```

**useLayoutEffect**는 useEffect와 사용법이 동일하며 거의 같은 방식으로 동작해요.

**컴포넌트가 업데이트된 직후 화면이 렌더링되기 전에 실행된다는 것**에서 차이점이 있어요.

화면을 렌더링하기 전에 변경할 부분이 있거나 수치 등을 측정해야 하는 상황에서 많이 사용돼ㅛ.

headerLeft함수의 파라미터에는 다양한 값들이 전달되는데, 그중 onPress는 뒤로 가기 버튼 기능이 전달돼요. 뒤로 가기 버튼의 기능을 그대로 기용하고 싶은 경우 유용하게 사용할 수 있어요.

headerRight 함수의 파라미터에는 tintColor만 전달되므로 onPress에 원하는 행동을 정의해줘야 하는데요.

navigation에서 제공하는 다양한 함수 중 **popToTop 함수**는 현재 쌓여 있는 모든 화면을 내보내고 첫 화면으로 돌아가는 기능이에요.

#### 헤더 감추기

**headerMode**는 Navigator 컴포넌트의 속성으로 헤더를 렌더링하는 방법을 설정하는 속성이에요.

- **float**: 헤더가 상단에 유지되며 하나의 헤더를 사용해요.
- **screen**: 각 화면마다 헤더를 가지며 화면 변경과 함께 나타나거나 사라져요.
- **none**: 헤더가 렌더링되지 않아요.

float는 iOS에서 볼 수 있는 방식이고, screen은 안드로이드에서 볼 수 있는 동작 방식이에요.

**headerShown**은 화면 옵션으로, Navigator 컴포넌트의 screenOptions에 설정하면 전체 화면의 헤더가 보이지 않도록 설정할 수 있어요.

Home 화면에 headerShown을 false로 잡아주고 Home 컴포넌트에서 최상위 부모 태그를 SafeAreaView 컴포넌트를 적용을 했다.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator8.png?raw=true"
  style="width: 50%;"
/>

여기서 번외로 SafeAreaView는 현재 Deprecated인 상태이다. 

##### Deprecated란?

당장은 사용 가능하지만 곧 없어질 기능을 말해요.

그래서 다른 방식을 쓰는 것을 추천을 한다는 말이에요.

**react-native-safe-area-context** 라이브러리를 설치해서 사용하라고 하는데, 이건 더 찾아보고 다음에 사용할 일이 있으면 그 때 설명과 함께 사용을 해볼게요!

## 탭 내비게이션

탭 내비게이션은 보통 화면 위나 아래에 위치하며, 탭 버튼을 누르면 버튼과 연결된 화면으로 이동하는 방식으로 동작해요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/image4.png?raw=true"
  style="width: 50%;"
/>

```bash
npm i @react-navigation/bottom-tabs
```

### 화면 구성

```jsx
// src/screens/TabScreens.js
import React from "react";
import styled from "styled-components/native";

const Container = styled.View`
  flex: 1;
  justify-content: center;
  align-items: center;
`;
const StyledText = styled.Text`
  font-size: 30px;
`;

export const Mail = () => {
  return (
    <Container>
      <StyledText>Mail</StyledText>
    </Container>
  );
};

export const Meet = () => {
  return (
    <Container>
      <StyledText>Meet</StyledText>
    </Container>
  );
};

export const Settings = () => {
  return (
    <Container>
      <StyledText>Settings</StyledText>
    </Container>
  );
};
```

```jsx
// src/navigations/Tab.js

import React from "react";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import { Mail, Meet, Settings } from "../screens/TabScreens";

const Tab = createBottomTabNavigator();

const TabNavigation = () => {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Mail" component={Mail} />
      <Tab.Screen name="Meet" component={Meet} />
      <Tab.Screen name="Settings" component={Settings} />
    </Tab.Navigator>
  );
};

export default TabNavigation;
```

위 코드들을 작성을 하고 결과를 확인 해보면 하단에 3개의 버튼이 놓인 탭 바가 있고, 탭의 버튼을 클릭할 때마다 화면이 변경이 돼요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator9.png?raw=true"
  style="width: 50%;"
/>

탭 바에 있는 버튼 순서는 Navigator 컴포넌트의 자식으로 있는 Screen 컴포넌트의 순서와 동일하며 첫 번째 자식 컴포넌트를 첫 화면으로 사용해요.

탭 버튼의 순서는 변경하지 않고 렌더링되는 첫 번째 화면을 변경하고 싶을 경우 **initialRouteName 속성**을 이용해요.

앞에서 했던 스택 내비게이션과 흐름이 비슷한 것 같아요!

### 탭 바 수정하기

#### 버튼 아이콘 설정하기

탭 버튼에 아이콘을 렌더링하는 방법은 **tabBarIcon**을 이용하는 것이에요.

tabBatIcon에 컴포넌트를 반환하는 함수를 지정하면 버튼의 아이콘이 들어갈 자리에 해당 컴포넌트를 렌더링해요.

tabBarIcon에 설정된 함수에는 color, size, focused값을 포함한 객체가 라마미터로 전달된다는 특징이 있어요.

```jsx
// src/navigations/Tab.js
...
import { MaterialCommunityIcons } from "@expo/vector-icons";

const TabIcon = ({ name, size, color }) => {
  return <MaterialCommunityIcons name={name} size={size} color={color} />;
};

const Tab = createBottomTabNavigator();

const TabNavigation = () => {
  return (
    <Tab.Navigator initialRouteName="Settings">
      <Tab.Screen
        name="Mail"
        component={Mail}
        options={{
          tabBarIcon: (props) => TabIcon({ ...props, name: "email" }),
        }}
      />
      <Tab.Screen
        name="Meet"
        component={Meet}
        options={{
          tabBarIcon: (props) => TabIcon({ ...props, name: "video" }),
        }}
      />
      <Tab.Screen
        name="Settings"
        component={Settings}
        options={{
          tabBarIcon: (props) => TabIcon({ ...props, name: "settings" }),
        }}
      />
    </Tab.Navigator>
  );
};

export default TabNavigation;
```

화면을 구성하는 Screen 컴포넌트마다 tabBarIcon에 MaterialCommunityIcons 컴포넌트를 반환하는 함수를 지정했어요.

반환되는 컴포넌트의 색과 크기는 tabBarIcon에 지정된 함수의 파라미터로 전달되는 color와 size를 이용해서 설정했어요.

만약 Screen 컴포넌트마다 탭 버튼 아이콘을 지정하지 않고 한 곳에서 모든 버튼의 아이콘을 관리하고 싶은 경우 Navigator 컴포넌트의 screenOptions 속성을 사용해서 관리할 수 있어요.

```jsx
// src/navigations/Tab.js
...
<Tab.Navigator
  initialRouteName="Settings"
  screenOptions={({ route }) => ({
    tabBarIcon: (props) => {
      let name = "";
      if (route.name === "Mail") name = "email";
      else if (route.name === "Meet") name = "video";
      else name = "settings";
      return TabIcon({ ...props, name });
    },
  })}
>
...
```
이런식으로 Screen 컴포넌트에 적는 게 아니라 Navigator 컴포넌트에서 한 번에 관리를 하도록 코드를 작성할 수 있어요.

어떤 방식으로 하는지는 프로젝트나 개인 취향에 따라 정하면 될 것 같아요.

근데 저는 왜  `WARN  "settings" is not a valid icon name for family "material-community"` 이렇게 나올까요?

그래서 검색해보니 "cog"가 톱니바퀴 아이콘이 나온다길래 수정했어요!

근데 이상하게 안드로이드에서는 아이콘 모두 나오지가 않네요..

<div style="display: flex; gap: 10px;">
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator10.png?raw=true"
    style="width: 50%;"
  />
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator11.png?raw=true"
    style="width: 50%;"
  />
</div>

#### 라벨 수정하기

버튼 아이콘 아래에 렌더링되는 라벨은 Screen 컴포넌트의 **name값을 기본값**으로 사용해요.

탭 버튼의 라벨은 **tabBarLabel**을 이용해서 변경할 수 있어요.

라벨을 버튼 아이콘의 아래가 아닌 아이콘 옆에 렌더링되도록 변경하고 싶으면 screenOption에서 **tabBarLabelPosition**의 값을 변경해서 조정할 수 있어요.

tabBarLabelPosition은 **below-icon**과 **beside-icon** 두 값만 설정할 수 있으며, beside-icon으로 설정하면 아이콘 오른쪽에 라벨이 렌더링 돼요

라벨 위치를 변경하는 부분에서 안드로이드는 또 안되네요?

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator12.png?raw=true"
  style="width: 50%;"
/>

**tabBarShowLabel**을 이용하면 탭 바에서 라벨이 렌더링되지 않도록 설정할 수 있어요.

#### 스타일 수정하기

탭 내비게이션의 탭 바 배경색은 흰색이 기본값이에요.

먼저 TabScreens.js에서 배경 색을 변경하고 Tab.js에서 Navigator 컴포넌트에서 스타일을 설정합니다. 스타일 지정은 screenOptions 안에 **tabBarItemStyle 속성** 으로 설정을 할 수 있어요.

탭 바의 배경색이 변경이 되었지만 탭 버튼과 아이콘 색이 어울리지 않아서 수정을 해요.
스타일 지정은 screenOptions 안에 **tabBarActiveTintColor**와 **tabBarInactiveTintColor**를 이용해 설정할 수 있어요.

앞에서 버튼의 아이콘을 설정하기 위해 barTabIcon에 설정한 함수에는 파라미터로 size, color, focused를 가진 객체가 전달돼요.

이 값 중 focused는 버튼의 선택된 상태를 나타내는 값인데, 이 값을 이용하면 버튼의 활성화 상태에 따라 다른 버튼을 렌더링하거나 스타일을 변경할 수 있어요.

focused의 값에 따라 버튼이 활성화되었을 때는 내부가 채워진 이미지가 렌더링되고, 비활성화 상태에서는 내부가 빈 아이콘이 렌더링되도록 작성했어요.

<div style="display: flex; gap: 10px;">
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator13.png?raw=true"
    style="width: 50%;"
  />
  <img 
    src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter8/simulator14.png?raw=true"
    style="width: 50%;"
  />
</div>

앞에서 설명했던 것들을 다 적용한 결과 화면이에요.

```jsx
// src/natigations/Tab.js
import React from "react";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";
import { Mail, Meet, Settings } from "../screens/TabScreens";
import { MaterialCommunityIcons } from "@expo/vector-icons";

const TabIcon = ({ name, size, color }) => {
  return <MaterialCommunityIcons name={name} size={size} color={color} />;
};

const Tab = createBottomTabNavigator();

const TabNavigation = () => {
  return (
    <Tab.Navigator
      initialRouteName="Settings"
      screenOptions={{
        tabBarLabelPosition: "beside-icon",
        tabBarItemStyle: {
          backgroundColor: "#54b7f9",
          borderTopColor: "#ffffff",
          borderTopWidth: 2,
        },
        tabBarActiveTintColor: "#ffffff",
        tabBarInactiveTintColor: "#cfcfcf",
      }}
    >
      <Tab.Screen
        name="Mail"
        component={Mail}
        options={{
          tabBarLabel: "Inbox",
          tabBarIcon: (props) =>
            TabIcon({
              ...props,
              name: props.focused ? "email" : "email-outline",
            }),
        }}
      />
      <Tab.Screen
        name="Meet"
        component={Meet}
        options={{
          tabBarIcon: (props) =>
            TabIcon({
              ...props,
              name: props.focused ? "video" : "video-outline",
            }),
        }}
      />
      <Tab.Screen
        name="Settings"
        component={Settings}
        options={{
          tabBarIcon: (props) =>
            TabIcon({ ...props, name: props.focused ? "cog" : "cog-outline" }),
        }}
      />
    </Tab.Navigator>
  );
};

export default TabNavigation;
```

전체 코드는 이렇게 됩니다!

왜인지 모르겠는데 iOS에서는 잘 적용이 되지만 안드로이드에는 적용이 되지 않는 것들이 많아요.

교재와 지금 버전이 달라서 옵션이나 속성들의 이름이 많이 다르기도 했는데, 그래서인지 달라진 부분이 많은 것 같아요.

