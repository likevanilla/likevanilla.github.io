---
layout: post
title: "Pygame 프로젝트 개발기 - 라이선스"
description: Github에서 라이선스를 적용하는 방법에 대해서 알아보도독 도도도독

categories: python
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}


* toc
{:toc}


## 들어가기 전....
> 라이선스랑 저작권이랑 비슷한줄 알았는데 생각보다 차이가 있다!
> 궁금하면 [눌러보기](https://designkit.tistory.com/entry/%EC%A0%80%EC%9E%91%EA%B6%8CCopyright%EA%B3%BC-%EB%9D%BC%EC%9D%B4%EC%84%A0%EC%8A%A4License%EB%9E%80)
{:.lead}


## 라이선스란 뭘까❔

내가 설명하는 것보다 안 쓴 사람은 있어도 한 번만 쓴 사람은 없다는 **CHAT GPT**에게 물어보았다.

### 소프트웨어 라이선스
- **프로프라이에터리 소프트웨어 라이선스**: 특정 기업이나 개인이 소유권을 가지며 사용자가 소프트웨어를 특정 조건에 따라 사용할 수 있도록 허가합니다. 
예: Windows, Adobe 제품.

- **오픈소스 라이선스**: 소프트웨어 소스 코드를 공개하여 누구나 사용, 수정, 배포할 수 있도록 허가합니다. 하지만 라이선스 유형에 따라 조건이 다를 수 있습니다.
대표적인 오픈소스 라이선스: MIT, GPL, Apache.

그렇다고 한다. . . 여기서 우리 팀은 **MIT 라이선스**를 채택하기로 했다!

MIT 라이선스에 대해서도 **CHAT GPT**에게 물어보았다.

### MIT LICENSE
- 자유로운 사용: 누구나 소스 코드를 복사, 수정, 배포, 상업적으로 이용할 수 있습니다.
- 책임 면책: 소프트웨어 제공자는 사용으로 인해 발생하는 문제에 대한 법적 책임을 지지 않습니다.
- 조건: 원래의 MIT 라이선스 텍스트와 저작권 표시를 소스 코드나 배포된 소프트웨어에 포함해야 합니다.

아주 잘 설명을 해준다.

프로젝트 할 때도 도움을 많이 줬던 녀석... 가끔은 도움이 안 돼서 다른 녀석을 찾아봤지만 결국 돌아온다..
{:.faded}

이제부턴 라이선스를 적용하는 방법에 대해 설명하겠다~!


## 적용하기

### 새 파일 생성
1. 라이선스를 적용하고싶은 레포지토리에 들어간다.

2. Add file -> Create new file

3. 파일 이름은 `LICENSE` 또는 `LICENSE.txt`로 정합니다.

### 내용

방법은 2가지가 있다. 둘 중 하나를 골라서 하면 된다.

#### 방법 1️⃣

~~~text
MIT License

Copyright (c) [연도] [저작권자 이름]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
~~~

위의 내용을 복사해서 붙여넣고 `[연도]` 부분에는 적용시키는 현재 연도를 입력하고, `[저작권자 이름]`에는 자신의 이름 또는 프로젝트 이름을 입력한다.

#### 방법 2️⃣
`Create new file`까지 한 뒤에 파일 이름을 `LICENSE`로 적으면 `Choose a license template` 버튼이 생긴다.

이 버튼을 누르면 여러가지 라이선스 관련해서 보여주고 선택해서 적용할 수 있다.

[연도]와 [저작권자 이름] 부분도 바로 수정하고 적용할 수 있다.

### 결과
위 방법들을 따르면 해당 레포지토리 메인 우측 About에 이런게 생긴다!

![About](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame3/about.png?raw=true)
<br>
결과 화면임당
MIT license가 생긴 게 보이죵?
{:.figcaption}

그리고 README.md 파일에 이런식으로 추가할 수도 있다.

![readme footer](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame3/readme_footer.png?raw=true)
<br>
README.md 하단(footer)임당
뭘 보고 했는진 기억이 잘 안 나네용
{:.figcaption}


레포지토리 메인에 README 옆에 이렇게 추가도 된다!

![main](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame3/main.png?raw=true)
<br>
README 옆에 MIT LICENSE가 생긴 모습임당
{:.figcaption}



> 자신이 아끼는 레포지토리라면 라이선스 적용을 해주면 좋을 것 같다.
> 다음 글은 기능 개발에 대해 적을 것이다!
{:.lead}