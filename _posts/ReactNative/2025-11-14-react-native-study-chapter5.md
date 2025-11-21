---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 3"
description: "[처음부터 배우는 리액트 네이티브] Chapter 5 할 일 관리 애플리케이션"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-11-17
last_modified_at: 2025-11-21
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

Input 컴포넌트에 입력되는 값을 이용해서 이벤트를 생성해볼게요.

```jsx
//src/App.js
import React, { useState } from "react";
...
export default function App() {
  const [newTask, setNewTask] = useState("");

  const _addTask = () => {
    alert(`Add: ${newTask}`);
    setNewTask("");
  };

  const _handleTextChange = (text) => {
    setNewTask(text);
  };
  ...
        <Input
          placeholder="+ Add a task"
          value={newTask}
          onChangeText={_handleTextChange}
          onSubmitEditing={_addTask}
        />
  ...
}
```
```jsx
// src/components/Input.js
import PropTypes from "prop-types";
...
const Input = ({ placeholder, value, onChangeText, onSubmitEditing }) => {
  const width = Dimensions.get("window").width;

  return (
    <StyledInput
      width={width}
      placeholder={placeholder}
      maxLength={50}
      autoCapitalize="none"
      autoCorrect={false}
      returnKeyType="done"
      keyboardAppearance="dark"
      value={value}
      onChangeText={onChangeText}
      onSubmitEditing={onSubmitEditing}
    />
  );
};

Input.propTypes = {
  placeholder: PropTypes.string,
  value: PropTypes.string.isRequired,
  onChangeText: PropTypes.func.isRequired,
  onSubmitEditing: PropTypes.func.isRequired,
};

export default Input;
```

코드를 위와 같이 수정합시다.

먼저 useState로 newTask 상태 변수에 Input 컴포넌트 값이 변할 때마다 저장되게 작성했어요.

입력을 하면 Input 컴포넌트를 공백으로 초기화되게 작성했어요.

Input 컴포넌트에서 props로 전달된 값들을 설정하고 `PropsTypes`를 이용해서 전달되는 값들의 타입과 필수 여부를 지정해요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter5/simulator_screen2.png?raw=true"
  style="width: 50%;"
/>

### 할 일 목록 만들기

우선 프로젝트에서 사용할 아이콘 이미지를 다운 받아야 해요.

[구글 머티리얼 다자인 아이콘(체크박스)](https://fonts.google.com/icons?selected=Material+Symbols+Outlined:check_box:FILL@0;wght@400;GRAD@0;opsz@48&icon.query=check+box&icon.size=72&icon.color=%231f1f1f&icon.platform=web)

여기서 **24**, **38**, **72** 사이즈로 총 3개의 PNG 파일을 저장했고, 각각 이름은 `check_box.png`, `check_box@2x.png`, `check_box@3x.png` 이렇게 설정을 해줬어요.

check_box 뒤에 **@2x**, **@3x** 이런식으로 지정한 이유는 리액트 네이티브에서 화면 사이즈에 알맞은 크기의 이미지를 자동으로 불러와서 사용하기 때문이에요.

그 뒤 파일은 assets/icons 에 저장해주세요. icons 폴더는 직접 만들어야 해요.

이런식으로 check box outline, delete forever, edit을 검색하고 저장해주세요.

#### IconsButton 컴포넌트

앞서 저장한 아이콘들을 이용해서 IconButton 컴포넌트를 만들거에요.

```jsx
// src/images.js
import CheckBoxOutline from "../assets/icons/check_box_outline.png";
import CheckBox from "../assets/icons/check_box.png";
import DeleteForever from "../assets/icons/delete_forever.png";
import Edit from "../assets/icons/edit.png";

export const images = {
  uncompleted: CheckBoxOutline,
  completed: CheckBox,
  delete: DeleteForever,
  update: Edit,
};
```

```jsx
// src/components/IconButton.js
import PropTypes from "prop-types";
import React from "react";
import { Pressable } from "react-native";
import styled from "styled-components/native";
import { images } from "../images";

const Icon = styled.Image`
  tint-color: ${({ theme }) => theme.text};
  width: 30px;
  height: 30px;
  margin: 10px;
`;

const IconButton = ({ type, onPressOut }) => {
  return (
    <Pressable
      onPressOut={onPressOut}
      style={({ pressed }) => [
        {
          opacity: pressed ? 0.3 : 1,
        },
      ]}
    >
      <Icon source={type} />
    </Pressable>
  );
};

IconButton.propTypes = {
  type: PropTypes.oneOf(Object.values(images)).isRequired,
  onPressOut: PropTypes.func,
};

export default IconButton;
```

```jsx
// src/App.js
      <Container>
        ...
        <IconButton type={images.uncompleted} />
        <IconButton type={images.completed} />
        <IconButton type={images.delete} />
        <IconButton type={images.update} />
      </Container>
```

이런식으로 파일 작성을 해주시면

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter5/simulator_screen3.png?raw=true"
  style="width: 50%;"
/>

이렇게 아이콘들이 잘 나올거에요.

참고로 저는 tint-color라는 속성이 Unknown property라고 VSCode 상에서 경고 문구가 나타나는데,
이는 VSCode에서 CSS 기준으로 보고 없는 속성이라고 판단해서 나타나는 현상이라고 해요. 빌드에는 문제가 없으니 그냥 진행하면 될 것 같아요.

IconButton 컴포넌트에서 TouchableOpacity를 Pressable로 바꿔 주었어요.

TouchableOpacity는 레거시이고 RN 공식적으로도 Pressable 사용을 권장해요.

그래도 TouchableOpacity의 클릭시 반응이 확인하기에 좋기 때문에 스타일을 추가해줘서 비슷하게 만들어줬어요.

그리고 margin 값을 줘서 아이콘보다 큰 범위를 클릭해도 눌리게 해줬어요.

#### Task 컴포넌트

```jsx
// src/components/Task.js
import React from "react";
import styled from "styled-components";
import PropTypes from "prop-types";
import IconButton from "./IconButton";
import { images } from "../images";

const Container = styled.View`
  flex-direction: row;
  align-items: center;
  background-color: ${({ theme }) => theme.itemBackground};
  border-radius: 10px;
  padding: 5px;
  margin: 3px 0px;
`;

const Contents = styled.Text`
  flex: 1;
  font-size: 24px;
  color: ${({ theme }) => theme.text};
`;

const Task = ({ text }) => {
  return (
    <Container>
      <IconButton type={images.uncompleted} />
      <Contents>{text}</Contents>
      <IconButton type={images.update} />
      <IconButton type={images.delete} />
    </Container>
  );
};

Task.PropTypes = {
  text: PropTypes.string.isRequired,
};

export default Task;
```

```jsx
// src/App.js
...
import Task from "./components/Task";
...

const List = styled.ScrollView`
  flex: 1;
  width: ${({ width }) => width - 40}px;
`;

export default function App() {
  const width = Dimensions.get("window").width;
  ...
  return (
    <ThemeProvider theme={theme}>
      <Container>
        ...
        <List width={width}>
          <Task text="밥먹기" />
          <Task text="React Native" />
          <Task text="React Native Sample" />
          <Task text="Edit TODO Item" />
        </List>
      </Container>
    </ThemeProvider>
  );
}
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter5/simulator_screen4.png?raw=true"
  style="width: 50%;"
/>

Task 컴포넌트를 만들어서 완료 여부를 확인하는 버튼, 할 일 내용, 항목 삭제 버튼, 수정 버튼을 표현했어요.

할 일 내용은 props로 전달되어 오는 값을 활용했고, 완료 여부를 나타내는 체크 박스와 수정, 삭제 버튼을 IconButton 컴포넌트를 이용해서 만들었어요.

리액트 네이티브에서 제공하는 **ScrollView 컴포넌트**를 이용해서 할 일 항목의 수가 많아져서 화면을 넘어가도 스크롤을 이용할 수 있도록 했어요.

그리고 화면에 맞게 Dimensions로 너비를 구하고 -40px로 좌우공백을 주었어요.

### 기능 구현하기

#### 추가 기능

할 일 추가 기능을 추가할게요.

```jsx
// src/App.js
import React, { useState } from "react";
import styled, { ThemeProvider } from "styled-components/native";
import { theme } from "./theme";
import { Dimensions, StatusBar } from "react-native";
import Input from "./components/Input";
import IconButton from "./components/IconButton";
import { images } from "./images";
import Task from "./components/Task";

const Container = styled.SafeAreaView`
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

const List = styled.ScrollView`
  flex: 1;
  width: ${({ width }) => width - 40}px;
`;

export default function App() {
  const width = Dimensions.get("window").width;
  const [newTask, setNewTask] = useState("");
  const [tasks, setTasks] = useState({
    1: { id: "1", text: "밥먹기", completed: false },
    2: { id: "2", text: "React Native", completed: true },
    3: { id: "3", text: "React Native Sample", completed: false },
    4: { id: "4", text: "Edit TODO Item", completed: false },
  });

  const _addTask = () => {
    const ID = Date.now().toString();
    const newTaskObject = {
      [ID]: { id: ID, text: newTask, completed: false },
    };
    setNewTask("");
    setTasks({ ...tasks, ...newTaskObject });
  };

  const _handleTextChange = (text) => {
    setNewTask(text);
  };

  return (
    <ThemeProvider theme={theme}>
      <Container>
        <StatusBar
          barStyle="light-content"
          backgroundColor={theme.background}
        />
        <Title>TODO List</Title>
        <Input
          placeholder="+ Add a task"
          value={newTask}
          onChangeText={_handleTextChange}
          onSubmitEditing={_addTask}
        />
        <List width={width}>
          {Object.values(tasks)
            .reverse()
            .map((item) => (
              <Task key={item.id} text={item.text} />
            ))}
        </List>
      </Container>
    </ThemeProvider>
  );
}
```

할 일 목록은 내용, 완료 여부, 고유값을 가져야 하므로 딕셔너리 형태로 useState를 이용해서 목업 데이터를 저장해줘요.

최신 항목이 가장 앞에 보이도록 tasks를 역순으로 렌더링되게 작성해요.

id는 할 일 항목이 추가되는 시간의 타임스탬프를 이용하고 내용을 나타내는 text는 Input 컴포넌트에 입력된 값을 지정해요.

새로 입력되는 항목이므로 완료 여부는 false를 기본값으로 해둬요.

추가를 하면 기존 목록에서 새로운 항목이 추가되도록 했어요.

#### 삭제 기능

```jsx
// src/App.js
...
export default function App() {
  ...

  const _deleteTask = (id) => {
    // const currentTasks = Object.assign({}, tasks);
    const currentTasks = { ...tasks };
    delete currentTasks[id];
    setTasks(currentTasks);
  };

  ...

  return (
    <ThemeProvider theme={theme}>
      <Container>
        ...
        <List width={width}>
          {Object.values(tasks)
            .reverse()
            .map((item) => (
              <Task key={item.id} item={item} deleteTask={_deleteTask} />
            ))}
        </List>
      </Container>
    </ThemeProvider>
  );
}
```
```jsx
// src/components/Task.js
...
const Task = ({ item, deleteTask }) => {
  return (
    <Container>
      <IconButton type={images.uncompleted} />
      <Contents>{item.text}</Contents>
      <IconButton type={images.update} />
      <IconButton type={images.delete} id={item.id} onPressOut={deleteTask} />
    </Container>
  );
};

Task.PropTypes = {
  item: PropTypes.string.isRequired,
  deleteTask: PropTypes.func.isRequired,
};

export default Task;
```

```jsx
// src/componentsIconButton.js
...
const IconButton = ({ type, onPressOut, id }) => {
  const _onPressOut = () => {
    onPressOut(id);
  };
  return (
    <Pressable
      onPressOut={_onPressOut}
      style={({ pressed }) => [
        {
          opacity: pressed ? 0.3 : 1,
        },
      ]}
    >
      <Icon source={type} />
    </Pressable>
  );
};

IconButton.defaultProps = {
  onPressOut: () => {},
};

IconButton.propTypes = {
  type: PropTypes.oneOf(Object.values(images)).isRequired,
  onPressOut: PropTypes.func,
  id: PropTypes.string,
};

export default IconButton;
```

삭제 버튼을 클릭했을 때 항목의 id를 이용하여 tasks에서 해당항목을 삭제하는 _deleteTask 함수.

_deleteTask 함수에 있는 `const currentTasks = Object.assign({}, tasks);`이 함수는 _addTask 함수에서 사용한 스프레드로 똑같이 구현이 가능해요. 더 깔끔하게 보이기 때문에 기존 코드를 주석처리를 하고 수정한 코드로 진행을 했어요.
흐름은 tasks 객체를 복사 -> 해당 ID의 task 삭제 -> 새 상태로 업데이트

Task 컴포넌트에 생성된 항목 삭제 함수와 함께 항목 내용 전체를 전달해 자식 컴포넌트에서도 항목의 id를 확인할 수 있도록 수정.

props로 전달된 deleteTask 함수는 삭제 버튼으로 전달, 함수에서 필요한 항목의 id도 함께 전달.

props로 onPressOut이 전달되지 않았을 경우에도 문제가 발생하지 않도록 defaultProps를 이용해서 기본값 지정.

props로 전달되는 값들의 propTypes를 지정하고 IconButton 컴포넌트가 클릭되었을 때 함수가 호출되도록 작성.

#### 완료 기능

```jsx
// src/App.js
...

export default function App() {
  ...
  const _toggleTask = (id) => {
    const currentTasks = { ...tasks };
    currentTasks[id]["completed"] = !currentTasks[id]["completed"];
    setTasks(currentTasks);
  };
  ...
  return (
    <ThemeProvider theme={theme}>
      <Container>
        ...
        <List width={width}>
          {Object.values(tasks)
            .reverse()
            .map((item) => (
              <Task
                key={item.id}
                item={item}
                deleteTask={_deleteTask}
                toggleTask={_toggleTask}
              />
            ))}
        </List>
      </Container>
    </ThemeProvider>
  );
}
```

```jsx
// src/components/Task.js
...
const Contents = styled.Text`
  flex: 1;
  font-size: 24px;
  color: ${({ theme, completed }) => (completed ? theme.done : theme.text)};
  text-decoration-line: ${({ completed }) =>
    completed ? "line-through" : "none"};
`;

const Task = ({ item, deleteTask, toggleTask }) => {
  return (
    <Container>
      <IconButton
        type={item.completed ? images.completed : images.uncompleted}
        id={item.id}
        onPressOut={toggleTask}
        completed={item.completed}
      />
      <Contents completed={item.completed}>{item.text}</Contents>
      {item.completed || <IconButton type={images.update} />}
      <IconButton
        type={images.delete}
        id={item.id}
        onPressOut={deleteTask}
        completed={item.completed}
      />
    </Container>
  );
};

Task.PropTypes = {
  item: PropTypes.string.isRequired,
  deleteTask: PropTypes.func.isRequired,
  toggleTask: PropTypes.func.isRequired,
};

export default Task;
```

```jsx
// src/components/IconButton.js
...

const Icon = styled.Image`
  tint-color: ${({ theme, completed }) =>
    completed ? theme.done : theme.text};
  width: 30px;
  height: 30px;
  margin: 10px;
`;

const IconButton = ({ type, onPressOut, id, completed }) => {
  ...
  return (
    <Pressable
      onPressOut={_onPressOut}
      style={({ pressed }) => [
        {
          opacity: pressed ? 0.3 : 1,
        },
      ]}
    >
      <Icon source={type} completed={completed} />
    </Pressable>
  );
};
...
IconButton.propTypes = {
  type: PropTypes.oneOf(Object.values(images)).isRequired,
  onPressOut: PropTypes.func,
  id: PropTypes.string,
  completed: PropTypes.bool,
};

export default IconButton;
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter5/simulator_screen5.png?raw=true"
  style="width: 50%;"
/>

화면이 이렇게 잘 나오나요?

삭제 기능과 비슷하게 값들을 전달하고 아이콘의 이미지를 변경하고 할 일 내용에 text-decoration-line을 적용해서 완료된 일은 취소선을 긋고 색상도 바꿔줬어요.

그리고 수정 버튼이 렌더링되지 않도록 했어요.

#### 수정 기능

이번에는 수정 기능이에요.

```jsx
// src/App.js
...
export default function App() {
  ...

  const _updateTask = (item) => {
    const currentTasks = { ...tasks };
    currentTasks[item.id] = item;
    setTasks(currentTasks);
  };

  ...

  return (
    <ThemeProvider theme={theme}>
      <Container>
        ...
        <List width={width}>
          {Object.values(tasks)
            .reverse()
            .map((item) => (
              <Task
                key={item.id}
                item={item}
                deleteTask={_deleteTask}
                toggleTask={_toggleTask}
                updateTask={_updateTask}
              />
            ))}
        </List>
      </Container>
    </ThemeProvider>
  );
}
```

```jsx
// src/components/Task.js
...
import Input from "./Input";

...

const Task = ({ item, deleteTask, toggleTask, updateTask }) => {
  const [isEditing, setIsEditing] = useState(false);
  const [text, setText] = useState(item.text);

  const _handleUpdateButtonPress = () => {
    setIsEditing(true);
  };

  const _onSubmitEditing = () => {
    if (isEditing) {
      const editedTask = { ...item, text };
      setIsEditing(false);
      updateTask(editedTask);
    }
  };

  return isEditing ? (
    <Input
      value={text}
      onChangeText={(text) => setText(text)}
      onSubmitEditing={_onSubmitEditing}
    />
  ) : (
    <Container>
      ...
      <Contents completed={item.completed}>{item.text}</Contents>
      {item.completed || (
        <IconButton
          type={images.update}
          onPressOut={_handleUpdateButtonPress}
        />
      )}
      ...
    </Container>
  );
};

Task.PropTypes = {
  item: PropTypes.string.isRequired,
  deleteTask: PropTypes.func.isRequired,
  toggleTask: PropTypes.func.isRequired,
  updateTask: PropTypes.func.isRequired,
};

export default Task;
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter5/simulator_screen6.png?raw=true"
  style="width: 50%;"
/>

이제 할 일 내용을 수정할 수 있게 됐어요!

수정하는 함수 _updateTask를 만들고 이전과 동일하게 전달을 해요.

수정 버튼을 누르면 항목의 현재 내용을 가진 Input 컴포넌트가 렌더링되어 사용자가 수정할 수 있게 했어요.

수정 상태를 관리하기 위해 isEditing 변수를 생성하고 수정 버튼이 클릭되면 값이 변하도록 했어요.

수정되는 내용을 담을 text 변수를 생성하고 Input 컴포넌트의 값으로 설정했어요.

화면은 isEditing의 값에 따라 항목 내용이 아닌 Input 컴포넌트가 렌더링되도록 수정되었고, Input 컴포넌트에서 완료 버튼을 클릭하면 App 컴포넌트에서 전달된 updateTask 함수가 호출되도록 했어요.

#### 입력 취소하기

내용을 수정하려는 도중에 취소하고 싶을 수 있기 때문에 수정 취소 기능을 추가해불게요.

```jsx
// src/components/Input.js
...

const Input = ({
  placeholder,
  value,
  onChangeText,
  onSubmitEditing,
  onBlur,
}) => {
  const width = Dimensions.get("window").width;

  return (
    <StyledInput
      ...
      onSubmitEditing={onSubmitEditing}
      onBlur={onBlur}
    />
  );
};

Input.propTypes = {
  ...
  onBlur: PropTypes.func.isRequired,
};

export default Input;
```

```jsx
// src/App.js
...
export default function App() {
  ...
  const _onBlur = () => {
    setNewTask("");
  };

  return (
    <ThemeProvider theme={theme}>
      <Container>
        ...
        <Input
          placeholder="+ Add a task"
          value={newTask}
          onChangeText={_handleTextChange}
          onSubmitEditing={_addTask}
          onBlur={_onBlur}
        />
        ...
      </Container>
    </ThemeProvider>
  );
}
```

```jsx
// src/components.Task.js
...
const Task = ({ item, deleteTask, toggleTask, updateTask }) => {
  ...
  const _onBlur = () => {
    if (isEditing) {
      setIsEditing(false);
      setText(item.text);
    }
  };

  return isEditing ? (
    <Input
      value={text}
      onChangeText={(text) => setText(text)}
      onSubmitEditing={_onSubmitEditing}
      onBlur={_onBlur}
    />
  ) : (
    ...
  );
};
...
```

이렇게 코드를 수정하면 할 일 내용을 수정하는 도중에 할 일을 추가하는 Input으로 포커스를 바꾸면 수정 중이던 할 일이 수정 버튼을 누르기 전의 내용으로 돌아가요.

빈 화면을 눌러도 수정이 취소가 된다는데 iOS 기준으로 저는 안되네요....

### 부가 기능

교재에 나와있는 부가 가능들을 구현하려고 열심히 따라해보고 오류들을 수정하면서 해봤지만.. 자료가 오래돼서 그런지 잘 안 되는 것들이 많았어요.. 그래서 나중에 최신 정보들을 가지고 해봐야겠어요.


## 마치며

다음 글부터는 작성 방식을 바꿔야겠어요.. 이렇게 다 하려고 하니 시간도 오래걸리고 효율도 안 좋은 것 같아서 느낀점, 괜찮은 내용 등등을 가지고 글을 작성해볼게요!