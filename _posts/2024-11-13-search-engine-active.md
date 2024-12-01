---
layout: post
title: "[Github] Github Blog를 검색 엔진에 노출시키기"
description: Naver나 Google에 내 Blog를 검색하면 나오게! 

categories: devlog
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}


## WARNING!
> 이 글은 chirpy 테마를 기준으로 작성된 글이며 현재는 Hydejack 테마로 바꾼 상태라 원하시는 정보와 다를 수 있습니다! 
{:.lead}




# Github Blog 검색 엔진에 노출시키기

> 관심을 받기 위한 첫번째 발걸음.... 바로 검색 엔진에 노출!
> 이번 포스팅에서는 Naver와 Google 검색 엔진에 나의 Github Blog를 검색하면 결과에 나올 수 있게 설정해주는 방법에 대해 설명을 할 겁니당.

비교적 쉽다고 생각했던 **Naver**부터 알려드릴게요.

&nbsp;

## Naver
1. [Naver Search Advisor](https://searchadvisor.naver.com/) 접속

2. 네이버 로그인 및 이용약관 동의

3. `웹마스터 도구 사용하기 ->` 클릭

4. 나의 Github Blog URL 입력하고 등록하기

5. `HTML 태그` 체크

6. 아래의 메타 태그 복사 후 `_includes/head.html` 경로에 들어가서 **다른 코드에 영향이 없는 곳**에 붙여넣기

7. `소유확인` 클릭

8. 사이트맵 제출하기  
    - `요청` - `사이트맵 제출` - 사이트맵 URL 입력 - `확인`
    - 사이트맵 URL 예시: https://user_name.github.io/sitemap.xml

&nbsp;

#### Naver 마무리
~~이렇게 하면 Naver에서 검색이 됩니다!~~  
사실 당장 검색이 되지는 않고 Naver에서 검사(?)를 한 뒤에 검색이 된다고 합니다..  
저는 오전에 제출했는데 글을 작성하는 지금 검색해보니 잘 나오네요!   
한국 기업이라 그런지 Google과는 다르게 빠르네요 @_@ ~~시차때문인가?~~

&nbsp;

## Google

1. [Google Search Console](https://search.google.com/search-console/about) 접속

2. Google 로그인

3. `시작하기` 클릭

4. Github Blog URL 입력 후 `계속` 클릭

5. 소유권 확인에서 `HTML 태그` 클릭
    - 아래 메타 태그 복사
    - `_config.yml` - webmaster_verification: - google: 찾기
    - 메타 테그 내용에서 `content=` 이후 "" 안의 내용만 붙여넣기
    - `확인` 클릭

6. `속성으로 이동`

7. 사이트맵 제출하기  
    - `Sitemaps` - 새 사이트맵 추가 - 사이트맵 URL 입력 - `제출`
    - 사이트맵 URL 예시: https://user_name.github.io/sitemap.xml

&nbsp;

#### Google 마무리
~~이렇게 하면 Google에서 검색이 됩니다!~~  
Naver와 동일하게 검사를 거치고 통과가 된다고 합니다..  
오전에 Naver와 Google을 동시에 진행했는데 Google은 아직 변화가 없네욧  
활성화가 되면 글 업데이트 하겠습니다! 그럼 업데이트 날짜랑 포스팅된 날짜랑 비교해서 얼마나 걸렸는 지 대충 알 수 있겠죠?!