---
layout: post
title: "Pygame 프로젝트 개발기 - 마무리 후기"
description: 프로젝트가 끝난 뒤 상호 평가가 이루어졌다. 그에 대한 Q&A

categories: python
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}


* toc
{:toc}


## 들어가기 전....
> 상호 평가를 보고 매우 뿌듯했다. 열심히 한 보람이 있었고 우여곡절이 있어서 힘들었지만 결과를 보고나니 힘들었던 만큼 기쁨이 컸다.
{:.lead}

## 상호평가 1
![상호평가1](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame9/mutual evaluation1.jpg?raw=true)

일단 전체적으로 호평이 너무 많았다! 읽어도 읽어도 기분이 좋아지는 평가였다.

##### Q. 방향키를 계속 누르고 있을 때 연속적으로 움직이는 움직임이 없어서 조금은 아쉬웠습니다.

A. 아마 오른쪽 방향키를 누른 상태에서 왼쪽 방향키를 누르면 가만히 서있는 상태를 말씀하신 것 같다. 나도 이 부분이 조금 마음에 걸렸지만.. 개선사항으로 내놓지는 않았다. 게임에 큰 문제가 되지는 않으니까! 

##### Q. START, QUIT, RE? 등의 글씨가 픽셀 폰트가 되는 것

A. 버튼을 제작할 때 이미지로 제작해서 가져온 픽셀 폰트를 적용하지는 못했습니다.. 아쉬운 부분이긴 하네용

##### Q. 음량 조절 옵션

A. 처음 게임을 실행했을 때 소리가 클 수도 있겠다고 생각해서 기본 볼륨을 많이 낮춰뒀어요. 조절 옵션을 만든다면 시작 인트로 화면, 종료 화면, PAUSE 화면에 만들면 좋겠네용

##### Q. 무적 시간일 때와 폭탄에 맞았을 때 시각적 이펙트

A. 무적 시간은 약 5초 정도로 시간을 보면서 플레이 하면 어느정도 유추는 가능! 그래도 따로 보이는 게 있으면 게임이 더 쉬워지겠네요
폭탄을 맞았을 때 폭탄이 터지는 이미지도 추가하려는 의견이 있었지만 밀리다보니 따로 추가는 하지않은.. 있으면 좋을 것 같다고 생각은 들어요
현재는 목숨이 깎이는 부분, 충돌 소리, 사라지는 폭탄으로 판별할 수 있어요!

##### Q. 일시정지 중일 때 배경음의 변화

A. 저도 처음에 일시정지가 됐을 때 배경음악이 계속 나오길래 의아했는데 팀원분들은 계속 나오는 게 더 좋은 것 같다고 해서 그냥 계속 나오게 뒀습니다!

##### Q. 아이템 설명이나 효과가 잘 보이게

A. 아이템 설명을 README에 적어놨으나 상세하게 적어두지는 않았습니다. 그게 조금 더 README를 읽었을 때 가독성이 좋아보이고 읽기 편할 것 같아보였습니다.
효과는 따로 없지만 플레이를 해보면 느껴집니다!
게임 화면에 이동속도 등을 표기하는 것도 괜찮을 것 같네요.

## 상호평가2
![상호평가2](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame9/mutual evaluation2.jpg?raw=true)

##### Q. 소리를 켜지 않고 플레이 했을 때 목숨 변화에 대해

A. 왼쪽 상단에 목숨 표시가 있고 폭탄도 충돌하면 사라지기 때문에 어느정도 확인이 된다고 생각했는데 위에 있던 피드백과 비슷하게 시각적 이펙트가 필요하다는 내용이네요.
확실히 시각적 효과가 있는 게 좋겠군요...

##### Q. PR 변경 내용이 부실함

A. 아무래도 팀원 전부 Git 협업이 처음이라 모든 것이 완벽하지는 못했던 것 같습니다.


## 진짜진짜 마무리

상호평가의 만점은 5점.. 그 중에서 4.4점을 평균으로 받았다.

아마도 제일 높은 점수이지 않을까??ㅎ_ㅎ

이러한 팀 프로젝트가 나름 잘 맞았다는 느낌이 들었다. 물론 첫 프로젝트라서 아직 모르겠지만 아직까지는 나에게 좋은 느낌을 준다.