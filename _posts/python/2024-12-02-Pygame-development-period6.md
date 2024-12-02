---
layout: post
title: "Pygame 프로젝트 개발기 - 캐릭터 및 오브젝트 이미지 변경"
description: 밋밋한 캐릭터와 오브젝트의 이미지를 바꿔보자!

categories: python
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}


* toc
{:toc}


## 들어가기 전....
> 짜치는 캐릭터와 오브젝트 이미지.. 심지어 캐릭터는 이미지 그대로 수평이동을 한다..
> 스크래치도 이것보단 역동적이고 재밌을듯 싶다ㅋㅎㅋㅎ
> **그 래 서 !** 싹 다 바꾸고 캐릭터에 애니메이션을 추가해줄 것이다!!
{:.lead}

## 🎨 적당한 이미지 찾기 
생각보다 시간이 엄청 걸렸던 작업... 이미지 컨셉은 픽셀 아트로 정했다.

이미지를 찾을 때는 웬만해서 저작권 관련 문제가 없고 무료로 다운받을 수 있는 걸로 서칭을 했다. 

- [폭탄 이미지 출처](https://engineering15.itch.io/pixel-pack)

- [캐릭터 이미지 출처](https://snoblin.itch.io/pixel-rpg-free-npc)

![폭탄 이미지](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame6/bomb.png?raw=true)
<br>
폭탄 이미지임당
{:.figcaption}

<hr>

![캐릭터 이미지](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame6/Character.png?raw=true)
<br>
캐릭터 이미지팩임당
{:.figcaption}

캐릭터 이미지팩은 잘라서 써야하는데 주변 지인에게 포토샵을 부탁했다.. 좌우 반전까지.. 고오맙다..!!
{:.faded}

## 적용해보자!

### 💣 폭탄

폭탄 이미지 적용은 간단하다.

원래 있던 폭탄 이미지 파일을 새로 가져온 이미지로 바꾸기만 하면 적용 끝이다!

### 🏃‍♂️ 캐릭터

캐릭터의 경우에는 애니메이션 기능을 추가하기 때문에 코드를 추가하고 수정해줘야한다.

먼저 이런식으로 왼쪽으로 갈 때와 오른쪽으로 갈 때 그리고 가만히 있을 때의 이미지 변수를 따로 저장해준다.

```python
person_idle_image = pygame.image.load('bomb_game/img/person_idle.png').convert_alpha()
person_left_images = [
    pygame.image.load('bomb_game/img/person_left1.png').convert_alpha(),
    pygame.image.load('bomb_game/img/person_left2.png').convert_alpha()
]
person_right_images = [
    pygame.image.load('bomb_game/img/person_right1.png').convert_alpha(),
    pygame.image.load('bomb_game/img/person_right2.png').convert_alpha()
]

person_idle_image = pygame.transform.scale(person_idle_image, (100, 100))
person_left_images = [pygame.transform.scale(img, (100, 100)) for img in person_left_images]
person_right_images = [pygame.transform.scale(img, (100, 100)) for img in person_right_images]
```

양 방향으로 움직일 때는 두가지 이미지를 넣어서 걷는 느낌이 나게 표현을 하고싶었다. 그러기 위해서는 이런식으로 리스트로 저장해준다.

#### 🙋‍♂️ convert_alpha()란?
pygame에서 이미지의 배경을 투명하게 하고싶을 때 사용하는 메소드이다.

이미지의 확장자는 PNG와 같은 투명도를 지원하는 확장자를 사용할 때 적용시킬 수 있다.

#### 애니메이션 관련 변수
애니메이션 효과를 나타내기 위해선 코드 추가 및 수정이 필요하다고 위에서 말했다. 

```python
# 애니메이션 관련 변수
animation_index = 0  # 현재 애니메이션 프레임 인덱스
animation_speed = 0.2  # 애니메이션 속도 (작을수록 빠름)
animation_timer = 0  # 프레임 전환을 위한 시간 누적
person_image = person_idle_image  # 초기 이미지는 가만히 있는 상태
```

이미지가 바뀌는 것에 대한 변수들이다.

```python
global animation_index, animation_timer
```

전역 변수로도 추가해준다.

```python
person_image = pygame.image.load('bomb_game/img/person_idle.png')
moving = False
```

기존 캐릭터의 이미지를 새로 추가한 이미지 중 가만히 있을 때의 이미지로 변경한다.

그리고 움직임에 대한 변수를 추가한다.

```python
# 키 이벤트 처리
for event in pygame.event.get():
    if event.type == pygame.QUIT:
        done = True  # 게임 종료
        break
    elif event.type == pygame.KEYDOWN and not game_over:
        if event.key == pygame.K_LEFT:
            person_dx = -5  # 왼쪽 키를 누르면 왼쪽으로 이동
            moving = True
        elif event.key == pygame.K_RIGHT:
            person_dx = 5  # 오른쪽 키를 누르면 오른쪽으로 이동
            moving = True
    elif event.type == pygame.KEYUP:
        if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
            person_dx = 0  # 키를 떼면 이동 정지
            moving = False
```

`moving` 변수를 키 이벤트 처리 부분에 추가해서 움직이는 지 가만히 있는 지에 대해 업데이트한다.

```python
if moving:
    animation_timer += animation_speed
    if animation_timer >= 1:
        animation_timer = 0
        animation_index = (animation_index + 1) % len(person_left_images)
    if person_dx < 0:  # 왼쪽으로 이동 중
        person_image = person_left_images[animation_index]
    elif person_dx > 0:  # 오른쪽으로 이동 중
        person_image = person_right_images[animation_index]
else:
    person_image = person_idle_image  # 이동하지 않을 때는 기본 이미지
```

`moving`에 대해 조건문을 만들어서 방향에 맞는 이미지로 전환시켜준다.


> try-except 문법을 여기서 사용할 줄은 몰랐는데 이런 경우에 꽤 유용한 것 같다.
> 학교 컴퓨터에 마침! 재생 장치가 없어서 이러한 버그를 발견할 수 있었고 그에 따른 대처를 바로 할 수 있었다. 다행이야~
> 다음 글은 특수 폭탄 생성에 대해 작성할 것이다.
{:.lead}