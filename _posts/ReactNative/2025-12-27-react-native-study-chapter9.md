---
layout: post
title: "[React Native] 리액트 네이티브 정복하기! - 7-2"
description: "[처음부터 배우는 리액트 네이티브] Chapter 9 채팅 어플리케이션"

categories: reactnative
permalink: "/:categories/:year/:month/:day/:title"

published: true
toc: true

date: 2025-12-27
last_modified_at: 2025-12-27
---

* 목차
{:toc}

[처음 배우는 리액트 네이티브] 책으로 리액트 네이티브 공부하기!

## 들어가기 전...
이 글은 이전 글과 이어지는 글입니다 !

[이전 글](https://likevanilla.github.io/reactnative/2025/12/11/react-native-study-chapter9)

## 메인 화면

### 내비게이션

인증에 성공하면 AuthStack 내비게이션 대신 렌더링할 MainStack 내비게이션의 화면을 만들어야 해요.

MainStack 내비게이션은 채널 목록 화면과 프로필 화면으로 구성된 MainTab 내비게이션을 첫 번째 화면으로 가지며 그 외에 채널 생성 화면과 채널 화면으로 구성돼요.

#### MainStack 내비게이션

간단하게 screens 폴더에서 채널 생성 화면(ChannelCreation.js)과 채널 화면(Channel.js)을 만들고 navigations 폴더에 MainStack.js 파일을 작성해요.

새로 파일을 만들면 항상 각 폴더에 있는 index.js 파일의 내용을 수정해줘야 해요.

#### MainTab 내비게이션

일반적으로 Screen 컴포넌트에는 화면으로 사용될 컴포넌트를 지정하지만, 내비게이션도 결국 컴포넌트이기 때문에 화면으로 사용할 수 있어요.

MainTab 내비게이션을 구성하는 채널 목록 화면(ChannelList.js)과 프로필 화면(Profile.js)을 작성해요.

그리고 MainTab 내비게이션을 MainStack 내비게이션의 첫 번째 화면으로 렌더링되도록 수정해요.

```jsx
// src/navigations/MainStack.js
...
import MainTab from "./MainTab";
...
return(
    <Stack.Navigator
        initialRouteName="Main"
    ...
    >
        <Stack.Screen name="Main" component={MainTab} />
        ...
)
...
```

### 인증과 화면 전환

인증 상태에 따라 MainStack 내비게이션과 AuthStack 내비게이션을 렌더링하는 방법을 알아볼 거에요.

여러 곳에서 상태를 변경해야 할 경우 Context API를 이용하면 수월하게 전역적으로 관리할 수 있어요.

UserContext를 만들고 인증 상태에 따라 적절한 내비게이션이 렌더링되도록 만들어야 해요.

### 채널 생성 화면

#### 데이터베이스

파이어베이스에서 제공하는 파이어스토어는 NoSQL 문서 중심의 데이터베이스로 SQL 데이터베이스와 달리 테이블이나 행이 없고 컬렉션, 문서, 필드로 구성돼요.

컬렉션은 문서의 컨테이너 역할을 하며 모든 문서는 항상 컬렉션에 저장되어야 해요.

문서는 파이어스토어의 저장 단위로 값이 있는 필드를 가져요. 문서의 가장 큰 특징은 컬렉션을 필드로 가질 수 있다는 점이에요.

그리고 파이어스토어는 일반적인 데이터베이스와 달리 데이터베이스의 내용이 수정되면 실시간으로 변경된 내용을 알 수 있다는 특징을 갖고 있어요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter9/database_structure.png?raw=true"
/>

컬렉션과 문서는 항상 유일한 ID를 갖고 있어야 한다는 규칙이 있어요.

### 채널 목록 화면

#### FlatList 컴포넌트

지금까지 많은 양의 데이터를 렌더링할 때 ScrollView 컴포넌트를 이용해 화면이 넘어가도 스크롤이 생성되어 확인할 수 있도록 만들었어요.

ScrollView 컴포넌트와 같은 역할을 하는 컴포넌트로 리액트 네이티브에서 제공하는 FlatList 컴포넌트가 있어요.

두 컴포넌트 모두 많은 양의 데이터를 목록으로 보여주는 상황에서 자주 사용되고, 렌더링된 내용이 화면을 넘어가면 스크롤이 된다는 공통점이 있어요.

**ScrollView 컴포넌트**는 렌더링해야 하는 모든 데이터를 한번에 렌더링하므로 렌더링해야 하는 데이터의 양을 알고 있을 때 사용하는 것이 좋아요.
렌더링하는 데이터가 매우 많을 경우 한번에 모든 데이터를 렌더링하면 속도가 느려지고 메모리 사용량이 증가하는 등 성능이 저하된다는 문제가 있어요.

**FlatList 컴포넌트**는 화면에 적절한 양의 데이터만 렌더링하고 스크롤의 이동에 맞춰 필요한 부분을 추가적으로 렌더링하는 특징이 있어요. 이런 특징 덕분에 데이터의 길이가 가변적이고 양을 예측할 수 없는 상황에서 사용하기 좋아요.

FlatList 컴포넌트를 사용하려면 3개의 속성을 지정해야 해요.

먼저 렌더링할 항목의 데이터를 배열로 전달해야 하고, 전달된 배열의 항목을 이용해 항목을 렌더링하는 함수를 작성해야 해요. 마지막으로 각 항목에 키를 추가하기 위해 고유한 값을 반환하는 함수를 전달해야 해요.

#### windowSize

FlatList 컴포넌트에서 렌더링되는 데이터의 수는 windowSize 속성에 의해 결정돼요.

windowSize의 기본값은 21이고, 이 값은 현재 화면(1)과 현재 화면보다 앞쪽에 있는 데이터(10), 그리고 현재 화면보다 뒤쪽에 있는 데이터(10)를 의미해요.

windowSize의 값을 작은 값으로 변경하면 렌더링되는 데이터가 줄어들어 메모리의 소비를 줄이고 성능을 향상시킬 수 있지만, 빠르게 스크롤하는 상황에서 미리 렌더링되지 않은 부분은 순간적으로 빈 내용이 나타날 수 있다는 단점이 있어요.

#### React.memo

스크롤이 이동하면 windowSize 값에 맞춰 현재 화면과 이전 및 이후 데이터를 렌더링하는 것이 맞지만, 이미 렌더링된 항목도 다시 렌더링되는 것을 확인할 수 있어요.

**React.memo**를 이용하면 이런 비효율적인 반복 작업을 해결할 수 있어요.

6장에서 공부한 useMemo Hook 함수와 매우 흡사하지만, 불필요한 함수의 재연산을 방지하는 useMemo와 달리 React.memo는 컴포넌트의 리렌더링을 방지한다는 차이가 있어요.

React.memo를 이용해 컴포넌트를 감싸는 것으로 간단히 적용할 수 있어요.

#### moment 라이브러리

**moment 라이브러리**를 사용하면 시간 및 날짜와 관련된 함수를 쉽게 작성할 수 있어요.

자바스크립트에 내장된 함수를 이용하면 타임스탬프를 우리에게 익숙한 시간이나 날짜 형태로 변경할 수 있지만, 자바스크립트에서 제공하는 기능만으로 기능을 구현하다 보면 조건에 따라 굉장히 복잡해지고 생각하지 못한 버그들도 많이 생겨요.

```jsx
import moment from "moment";

const getDateOrTime = ts => {
    const now = moment().startOf('day');
    const target = moment(ts).startOf('day');
    return moment(ts).format(now.diff(target, 'days') > 0 ? 'MM/DD' : 'HH:mm');
};
```

이런식으로 타임스탬프를 보기 좋은 형식으로 변경하는 코드를 작성할 수 있어요.

### 채널 화면

#### 데이터베이스

파이어스토어의 문서가 보유한 특징 중 하나인 컬렉션을 가질 수 있다는 점을 활용하여 각 채널 문서에 messages 컬렉션을 만들면 메세지 데이터를 관리할 수 있어요.

<img 
  src="https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/ReactNative/chapter9/database_structure2.png?raw=true"
/>

#### GiftedChat 컴포넌트

메세지를 주고받는 화면은 일반적으로 모바일 화면과 스크롤 방향이 반대에요.

FlatList 컴포넌트에 inverted 속성이 있는데, 이 값에 따라 FlatList 컴포넌트를 뒤집은 것처럼 스크롤 방향이 변경돼요.

채팅 화면에서 사용할 수 있는 기능을 다양하게 제공하는 **react-native-gifted-chat 라이브러리**를 사용해요.

react-native-gifted-chat 라이브러리의 GiftedChat 컴포넌트는 다양한 설정이 가능하도록 많은 속성을 제공해요.

입력된 내용을 설정된 사용자의 정보 및 자동으로 생성된 ID와 함께 전달하는 기능뿐 아니라, 전송 버튼을 수정하는 기능이나 스크롤의 위치에 따라 스크롤 위치를 변경하는 버튼을 렌더링할 수도 있어요.