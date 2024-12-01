---
layout: post
title: "[Github] Github Blog에 댓글 기능 추가하기"
description: Github Blog 게시물에 댓글을 달 수 있을까? 

categories: devlog
permalink: '/:categories/:year/:month/:day/:title'


published: true
---
{% include hits.md %}


## Large quotes
> 이 글은 chirpy 테마를 기준으로 작성된 글이며 현재는 Hydejack 테마로 바꾼 상태라 원하시는 정보와 다를 수 있습니다!
> 테마 변경 이후 댓글 관련 서비스도 다른 방법으로 적용했으니 유의하세요!
{:.lead}



# Github Blog에서 댓글을 달 수 있게 만들기

### Commnet 기능을 추가해야하는 이유
- 게시물을 읽어본 다른 사람과의 소통과 피드백이 가능하다.
- 댓글을 통해서 추가적인 정보를 얻을 수 있고 다른 이용자들에게도 알릴 수 있다.

>등등등... 여러가지 장점도 있지만 물론 단점도 있다... 하지만 이런 부류의 >게시물에는 '**악플**'은 없을테니 걱정하지 않아도 될 것 같다~

&nbsp;
&nbsp;

### UTTERANCES 적용하기
>utterances는 무료로 댓글기능을 사용할 수 있게 해준다.
>추가로 댓글을 남기면 원하는 Label을 달아서 Github Issue에 올라오게도 >설정을 할 수가 있다.
>이 기능은 뒤에서 설명한다.

1. [https://utteranc.es/](https://utteranc.es/) 접속

2. repo: owner/repo 작성
예시: likevanilla/likevanilla.github.io

3. Issue title contains page pathname 체크

4. label (optional): 위에서 말한 label, issue 기능
    - 자신의 github.io 레포지토리에 들어간다.
    - `issue`에 들어간다.
    - `Labels` 클릭
    - `New label` 클릭
    - 원하는 이름으로 Label name 작성, 설명과 색상도 자유롭게 설정할 수 있다.

5. Envale Utterances 문단을 보면 아래 코드박스가 있을 것이다. 이 코드박스 안에서 먼저 `repo`와 `issue-term` 부분만 적용을 할 것이다.
    - _config.yml에서 comment: 부분을 찾는다.
    - utterances: 아래 `repo`와 `issue-term` 부분을 사이트에서 복사하고 붙여넣는다.
```
  utterances:
    repo: "likevanilla/likevanilla.github.io" # <gh-username>/<repo>
    issue-term: "pathname" # < url | pathname | title | ..>
```

6. 이번에는 `Copy` 버튼을 눌러서 전체를 복사한다.
    - _layout/post.html로 들어간다.
    - 마지막 줄에 붙여넣기를 해준다.

7. 좌측 `Sorce Control(소스 제어)` 선택

8. `+` 버튼을 클릭하여 변경 사항 추

9. 커밋 메세지 입력, 커밋 & 푸시

