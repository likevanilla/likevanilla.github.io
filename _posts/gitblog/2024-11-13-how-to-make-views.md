---
layout: post
title: "[Github] Github Blog에 조회수 기능 추가하기"
description: 몇 명의 사람이 내 게시물을 봤을까?

categories: gitblog
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}



* toc
{:toc}



## WARNING!
> 이 글은 chirpy 테마를 기준으로 작성된 글이며 현재는 Hydejack 테마로 바꾼 상태라 원하시는 정보와 다를 수 있습니다! 
{:.lead}


# Github Blog에 Views 표시하기

### Views란?
특정 게시물이나 콘텐츠가 사용자들에 의해 몇 번이나 열람되었는지를 나타내는 수치를 말한다.

> 사실상 지금은 쓸모없는 기능이지만 시간이 꽤 지났을 때 보면 재밌을 것 같다...  
 쓸모없는 이유는 현재 내 블로그는 주소창에 직접 입력해야 들어올 수 있기 때문이다.  
 다음 포스팅은 **Google**과 **Naver**에서 검색을 통해 들어올 수 있게 해주는   내용에 대해 포스팅 할 예정이다.

&nbsp;

### GoatCounter 적용하기
> Goatcounter는 조회수 외에도 다양한 분석을 해주고 보여준다.

1. [goatcounter.com](https://goatcounter.com) 접속

2. `Sign up`

3. **code** : goatcounter를 쓰면서 사용할 자신만의 코드

4. **site domain** : 자신의 github.io 주소
예) likevanilla.github.io

5. **Email address** : 자신이 사용하는 이메일 주소

6. **Password** : 로그인 할 때 사용할 비밀번호

7. **Fill in 9 here** : 당신은 로봇입니까?

8. `sign up`

이 과정을 수행하면 자신만의 goatcounter 사이트가 생성된다.

### ⚙️ Settings

1. `code.goatcounter.com`로 접속한다.
예) likevanilla.goatcounter.com

2. 우측 상단 `Settings` 클릭

3. `Site settings` - `Allow adding visitor counts on your wevsite` 체크

4. 아래로 스크롤 후 `Save` 클릭

&nbsp;

### 마무리

이러면 끝이다!! 생각보다 적용하는 게 간단했다.  
조회수 변화에 흥미가 있거나 통계나 들어오는 데이터들에 관심이 있는 분들에게는 보는 재미도 있을 것 같다. 통계는 Settings 1번 주소로 들어가면 볼 수 있다.  
생각보다 다양한 정보들을 알 수 있으니 아직은 검색을 통해 들어올 수 없으니까 주변 사람들에게 링크를 공유해서 통계의 변화를 확인해보자~