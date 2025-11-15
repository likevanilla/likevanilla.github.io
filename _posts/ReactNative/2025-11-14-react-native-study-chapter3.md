---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 1"
description: "[처음부터 배우는 리액트 네이티브] Chapter 3 컴포넌트"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-11-14
last_modified_at: 2025-11-14
---

[처음 배우는 리액트 네이티브] 책으로 리액트 네이티브 공부하기!

1장과 2장은 따로 진행 후 3장부터 블로그를 작성하기로 했다.

개발환경 구축하는 과정에 제대로 되지 않는 부분들도 있고 결국 해내지 못한 부분들이 있었다..

처음 nvm 설치가 막혀서

https://juntcom.tistory.com/222

이 글을 참고하여 설치했다!

안드로이드는 결국 개발환경 구축 실패함.. 일단 IOS로만 실습 해보기

### 컴포넌트

컴포넌트란 재사용할 수 있는 조립 블록으로 화면에 나타나는 UI 요소를 말한다!

View는 div와 비슷한 역할을 하는 컴포넌트

Fragment 컴포넌트는 그냥 아무런 기능 없이 하나의 부모로 나머지 요소들을 감싸서 반환할 때 사용하면 될듯

근데 얘는 `import {Fragment} from react` 해줘야돼서 귀찮으니까 그냥

`import React from react`하고 `<> </>` 이렇게 빈 괄호로 표현해도 됨

Fragment 컴포넌트의 단축 문법임!

대신 Fragment는 가짜 컨테이너이기 때문에 스타일 적용이 되지 않음

그저 자식들을 그룹화 원툴(?)

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/reactnative/chapter3/simulator_screen1.jpg?raw=true"
  style="width: 50%;"
/>

```jsx
export default function App() {
  const name = "Jeonghyuk";
  return (
    <View style={styles.container}>
      <Text style={styles.text}>
        {(() => {
          if (name === "Hanbit") return "My name is Hanbit";
          else if (name === "Jeonghyuk") return "My name is Jeonghyuk";
          else return "My name is React Native";
        })()}
      </Text>
      <StatusBar style="auto" />
    </View>
  );
}
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen2.jpg?raw=true"
  style="width: 50%;"
/>

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen3.jpg?raw=true"
  style="width: 50%;"
/>

name 값을 pudding으로 바꿈

```jsx
export default function App() {
  const name = "Jeonghyuk";
  return (
    <View style={styles.container}>
      <Text style={styles.text}>
        My name is {name === "Jeonghyuk" ? "pudding" : "React Native"}
      </Text>
      <StatusBar style="auto" />
    </View>
  );
}
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen4.jpg?raw=true"
  style="width: 50%;"
/>

참 값을 pudding으로 둬서 pudding이 나와버리는 모습ㅋㅋ

```jsx
export default function App() {
  const name = "Jeonghyuk";
  return (
    <View style={styles.container}>
      {name === "Jeonghyuk" && (
        <Text style={styles.text}>My name is Jeonghyuk</Text>
      )}
      {name !== "Jeonghyuk" && (
        <Text style={styles.text}>My name is not Jeonghyuk</Text>
      )}
      <StatusBar style="auto" />
    </View>
  );
}
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen5.jpg?raw=true"
  style="width: 50%;"
/>

JSX에서 null이나 undefined를 반환하는 경우가 있는데

교재에서는 null은 허용되지만 undefined는 오류가 발생한다고 나와있어서 실습을 해보니 둘 다 오류는 발생하지 않았다

```jsx
  return (
    <View style={styles.container}>
      <Text /* Comment */>Comment</Text>
      <Text
      // Comment
      >
        Comment
      </Text>
    </View>
  );
}
```

### 주석 사용법

- `{/* */}`
- 태그 안에서는 // 또는 /\* \*/

단축키 : Command + /

자세한 스타일링은 4장에서 하고 3장에서는 인라인 스타일링에 대해 알려줌

객체 형태로 입력해야하고 카멜표기법을 사용함

처음에 App.js에 export default App();이라고 적어서 오류가 났음

컴포넌트 함수를 보내야 하는데 App을 호출해서 나온 JSX(src/App.js의 return을 보내버린 것과 같음)를 보내버린 것.

```jsx
// src/App.js
import React from 'react';
import { Text, View, Button } from 'react-native';

const App = () => {
	return (
		<View
			style={{
				flex: 1,
				backgroundColor: '#fff',
				alignItems: 'center',
				justifyContent: 'center',
			}}
		>
			<Text style={{ fontSize: 30, marginBottom: 10 }}>Button Component</Text>
			<Button title="Button" onPress={() => alert('Click!!!')} />
		</View>
	);
};

export default App;

// App.js
import App from './src/App';

export default App;
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen6.jpg?raw=true"
  style="width: 50%;"
/>

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen7.jpg?raw=true"
  style="width: 50%;"
/>

Button 컴포넌트의 color 속성은 iOS와 안드로이드에서 차이가 있음. (Button 외에도)

그래서 iOS와 안드로이드 모두 확인하면서 개발하는 습관이 중요. 하지만 나는 지금 안드 시뮬이 안 돌아감! 5장부터 되게 해야지

iOS와 안드로이드에서 다른 모습으로 렌더링된다는 단점을 보안하기 위해 커스텀 컴포넌트를 만드는 방법이 있음

```jsx
// src/components/MyButton.js
import React from "react";
import { TouchableOpacity, Text } from "react-native";

const MyButton = () => {
  return (
    <TouchableOpacity>
      <Text
        style={{
          fontSize: 24,
          backgroundColor: "#3498db",
          padding: 16,
          margin: 10,
          borderRadius: 8,
        }}
        onPress={() => alert("Click!!!")}
      >
        My Button
      </Text>
      <Text style={{ color: "white", fontSize: 24 }}>My Button</Text>
    </TouchableOpacity>
  );
};

export default MyButton;

// src/App.js
import React from "react";
import { Text, View, Button } from "react-native";
import MyButton from "./components/MyButton";

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
      <Text style={{ fontSize: 30, marginBottom: 10 }}>
        My Button Component
      </Text>
      <MyButton />
    </View>
  );
};

export default App;

```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen8.jpg?raw=true"
  style="width: 50%;"
/>

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen9.jpg?raw=true"
  style="width: 50%;"
/>

### props

```jsx
// App.js
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
      <Text style={{ fontSize: 30, marginBottom: 10 }}>Props</Text>
      <MyButton title="Button" />
    </View>
  );
};

// src/components/MyButton.js
const MyButton = (props) => {
  console.log(props);
  return (...)
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/terminal_screen1.jpg?raw=true"
  style="width: 50%;"
/>

```jsx
// src/components/MyButton.js
const MyButton = (props) => {
  return (
    <TouchableOpacity
      style={{
        fontSize: 24,
        backgroundColor: "#3498db",
        padding: 16,
        margin: 10,
        borderRadius: 8,
      }}
      onPress={() => alert("Click!!!")}
    >
      <Text style={{ color: "white", fontSize: 24 }}>
        {props.children || props.title}
      </Text>
    </TouchableOpacity>
  );
};

// src/App.js
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
      <Text style={{ fontSize: 30, marginBottom: 10 }}>Props</Text>
      <MyButton title="Button" />
      <MyButton title="Button">Children Props</MyButton>
    </View>
  );
};
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen10.jpg?raw=true"
  style="width: 50%;"
/>

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen11.jpg?raw=true"
  style="width: 50%;"
/>

왜 children props가 출력이 되는지 모르겠어요…

똑같이 title=”Button”인데 첫번째 컴포넌트 이후의 같은 속성?들은 다 자식 컴포넌트?

결론: MyButton 태그 안에 값이 없으면 title값 출력, 값이 있으면 그 값을 출력. ex)Children Props

여기서 말하는 그 값이 자식 컴포넌트 느낌인듯

```jsx
<MyButton title="Button" />
<MyButton title="Button">Children Props</MyButton>
<MyButton title="Button">Third Props</MyButton>
<MyButton title="Button" />
```

defaultProps.. 나는 왜 안될까요?

propTypes도 안된다

### useState

```jsx
// src/App.js
import React from "react";
import { View } from "react-native";
import Counter from "./components/Counter";

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
      <Counter />
    </View>
  );
};

export default App;

// src/components/Counter.js
import React, { useState } from "react";
import { View, Text } from "react-native";
import MyButton from "./MyButton";

const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <View style={{ alignItems: "center" }}>
      <Text style={{ fontSize: 30, margin: 10 }}>{count}</Text>
      <MyButton title="+1" onPress={() => setCount(count + 1)} />
      <MyButton title="-1" onPress={() => setCount(count - 1)} />
    </View>
  );
};

export default Counter;

```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen12.jpg?raw=true"
  style="width: 50%;"
/>

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/simulator_screen13.jpg?raw=true"
  style="width: 50%;"
/>

```jsx
// Double
const Counter = () => {
  const [count, setCount] = useState(0);
  const [double, setDouble] = useState(0);

  return (
    <View style={{ alignItems: "center" }}>
      <Text style={{ fontSize: 30, margin: 10 }}>count: {count}</Text>
      <Text style={{ fontSize: 30, margin: 10 }}>double: {double}</Text>
      <MyButton
        title="+"
        onPress={() => {
          setCount(count + 1);
          setDouble(double + 2);
        }}
      />
      <MyButton
        title="-1"
        onPress={() => {
          setCount(count - 1);
          setDouble(double - 2);
        }}
      />
    </View>
  );
};
```

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter3/terminal_screen2.jpg?raw=true"
  style="width: 50%;"
/>

```jsx
// src/components/EventButton.js
import react from "react";
import { TouchableOpacity, Text } from "react-native";

const EventButton = () => {
  const _onPressIn = () => console.log("Press In !!!\n");
  const _onPressOut = () => console.log("Press Out !!!\n");
  const _onPress = () => console.log("Press !!!\n");
  const _onLongPress = () => console.log("Long Press !!!\n");

  return (
    <TouchableOpacity
      style={{
        backgroundColor: "#f1c40f",
        padding: 16,
        margin: 10,
        borderRadius: 8,
      }}
      onPressIn={_onPressIn}
      onPressOut={_onPressOut}
      onPress={_onPress}
      onLongPress={_onLongPress}
      delayLongPress={3000}
    >
      <Text style={{ color: "white", fontSize: 24 }}>Press</Text>
    </TouchableOpacity>
  );
};

export default EventButton;

// src/App.js
import React from "react";
import { View } from "react-native";
import EventButton from "./components/EventButton";

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
      <EventButton />
    </View>
  );
};

export default App;

```

### Pressable

Pressable 컴포넌트는 TouchableOpacity 컴포넌트를 대체함. Pressable을 권장한다는데.. 그럼 빨리좀 말해주시지

의도하지 않은 버튼을 클릭하거나 버튼을 클릭하는 것 자체가 어려운 상황을 해결하기 위해 버튼 모양보다 약간 떨어진 부분까지 이벤트가 발생 할 수 있도록 도와줌 그것이 **HitRect**,

**PressRect**는 버튼을 클릭했을 때 해당 버튼이 동작하지 않게 하기 위해 버튼을 누른 상태에서 얼마나 멀어져야 벗어났다고 판단하는지에 대한 영역

```jsx
import React from "react";
import { View, Text, Pressable } from "react-native";

const Button = (props) => {
  return (
    <Pressable
      style={{ padding: 10, backgroundColor: "#1abc9c" }}
      onPressIn={() => console.log("Press In")}
      onPressOut={() => console.log("Press Out")}
      onPress={() => console.log("Press")}
      onLongPress={() => console.log("Long Press")}
      delayLongPress={3000}
      pressRetentionOffset={{ bottom: 50, left: 50, right: 50, top: 50 }}
      hitSlop={50}
    >
      <Text style={{ padding: 10, fontSize: 30 }}>{props.title}</Text>
    </Pressable>
  );
};

const App = () => {
  return (
    <View
      style={{
        flex: 1,
        justifyContent: "center",
        backgroundColor: "#fff",
        alignItems: "center",
      }}
    >
      <Button title="Pressable" />
    </View>
  );
};

export default App;
```

> 다음 장부터는 좀 순서있고 조잡하지 않게 작성해야겠다..
> velog로 넘어갈까도 심각하게 고민해보자.. 너무 힘들다
