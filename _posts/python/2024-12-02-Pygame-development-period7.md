---
layout: post
title: "Pygame 프로젝트 개발기 - 특수 폭탄 생성"
description: 교수님이 주신 첫번째 구현 미션!

categories: python
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}


* toc
{:toc}


## 들어가기 전....
> 교수님께서 우리가 만든 게임을 해보시고 있으면 좋을 것 같은 기능들을 제안해주셨다. ~~재미 없다고 하시며..~~
> 종합적으로 아이템을 만들어달라는 말씀이셨다.
> 별, 신발, 시계, 특수 폭탄, 대각선 폭탄... 여기서 나는 폭탄 관련된 기능들을 맡게되었다.
{:.lead}

## ❔ 어떤 폭탄이 좋을까

교수님의 말씀을 듣자마자 '변종'이라는 단어가 떠올랐다.

특수한 능력이 있는 폭탄이기 때문에 그렇게 생각한 것 같다. 

그러면서 같이 떠오른 게임. 바로 아이작이라는 게임이다.

### 아이작? 그게 뭔데

![아이작](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame7/issac.png?raw=true)

<br>
아이작 주인공임당 ~~귀여움~~
{:.figcaption}

게임은 안 해봐도 이 캐릭터를 본 적은 있을 수 있다. 

이 게임에서는 '변종'이라는 시스템이 존재한다.

#### 변종? 그건 또 뭔데

![아이작 변종](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame7/issac_play.png?raw=true)
<br>
게임 플레이 중 나온 색이 다른 친구 ~~이거 찍으려고 오랜만에 게임을 켰다.~~
{:.figcaption}

보이는 것과 같이 생김새가 똑같지만 초록색으로 색이 다른 녀석을 '변종'이라고 부른다.

사진에 나온 변종은 지나가면 초록색 장판이 생기는데 그 장판을 캐릭터가 밟으면 체력이 닳는 매커니즘의 변종이다.

이런 식으로 여러가지 종류의 변종이 있고 그만큼 다른 특성을 가진 몬스터들이 존재한다.

죽을 때 주변에 폭발 데미지를 준다던지.. 체력이 더 많다던지.. 등등
{:.faded}

이 매커니즘을 pygame에 적용시켜서 할 생각을 처음으로 했다.

## 변종 시스템
아이작에서는 몬스터의 색을 바꿔서 변종을 표현했다.

그래서 나도 폭탄의 색을 변경해서 특성을 부여시킬 생각을 했지만, 그렇게 하면 너무 성의없어 보이기도 하고 어떤 특성을 가졌는 지 알 방법도 없고 직관적이지 못하다.

그래서 그냥 특성에 걸맞는 이미지를 구해서 적용하기로 했다.

### 변종의 특성
적용할 특성은 총 2가지가 있다.

#### 🐢 거북이
거북이는 느리다는 말과 잘 어울린다. 그래서 거북이와 캐릭터가 충돌하면 캐릭터의 이동속도를 느리게 만들 것이다.

[거북이 이미지 출처](https://zejbo32.itch.io/crappy-turtle-spritesheet)

![거북이](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame7/turtle.png?raw=true)

#### 💣 다이너마이트
다이너마이트는 폭탄보다 강하다(?)

사실 어떤 폭탄이느냐에 따라서 다이너마이트보다 강할 수 있지만.. 이미지만 봤을 때는 폭탄보다 다이너마이트가 더 세보인다.

그래서 다이너마이트가 캐릭터와 충돌하면 생명이 2개가 깎이게 만들 것이다.

[다이너마이트 이미지 출처](https://tumas81.itch.io/minerman-adventure)

![다이너마이트](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame7/dynamite.png?raw=true)


## 💻 작성 코드

이 특수 폭탄들도 이전 게시물의 이미지 적용처럼 변수를 추가해주었다.

추후에는 마스킹 및 충돌 범위에 대해 글을 작성할 것이다.
{:.faded}

```python
    # 특성 폭탄 관련 변수
    special_bombs = []
    special_bomb_interval = random.randint(7000, 10000) # 7~10초 사이 간격
    last_special_bomb_time = 0

(...)

    # 특성 폭탄 생성
if elapsed_time - last_special_bomb_time > special_bomb_interval:
    last_special_bomb_time = elapsed_time
    special_bomb_interval = random.randint(7000, 10000)
    bomb_type = random.choice(["slow", "damage"])
    image = slow_bomb_image if bomb_type == "slow" else damage_bomb_image  # 크기 조정된 이미지 사용
    rect = image.get_rect()  # 선택된 이미지 기준으로 Rect 생성
    rect.left = random.randint(0, size[0] - rect.width)
    rect.top = -100
    dy = random.randint(4, 8)
    special_bombs.append({'rect': rect, 'dy': dy, 'type': bomb_type, 'image': image})

# 특성 폭탄 이동 및 그리기
for bomb in special_bombs[:]:
    bomb['rect'].top += bomb['dy']
    if bomb['rect'].top > size[1]:
        special_bombs.remove(bomb)
    else:
        screen.blit(bomb['image'], bomb['rect'])  # 크기 조정된 이미지를 화면에 그림

# 특성 폭탄 충돌 처리
for bomb in special_bombs[:]:
    offset = (bomb['rect'].left - person.left + 10, bomb['rect'].top - person.top + 10)
    if person_dx == 0:
        collision = check_collision(person_idle_mask, bomb['type'] == "slow" and slow_mask or damage_mask, offset)
    elif person_dx < 0:
        collision = check_collision(person_left_masks[animation_index], bomb['type'] == "slow" and slow_mask or damage_mask, offset)
    else:
        collision = check_collision(person_right_masks[animation_index], bomb['type'] == "slow" and slow_mask or damage_mask, offset)
    if collision:
        if bomb['type'] == "slow":
            person_speed = max(person_speed - 0.2, 0.2)
        elif bomb['type'] == "damage":
            lives -= 2
            if lives <= 0:
                game_over = True
        special_bombs.remove(bomb)
```

이런식으로 특수 폭탄의 생성 및 충돌 처리를 해주었다.

다음은 대각선 폭탄에 대해 설명하겠다.

## ❔ 대각선 폭탄
말 그대로 대각선으로 떨어지는 폭탄을 말한다.

처음에는 이 폭탄이 벽에 닿았을 때 반대 방향으로 튕겨지게 설계를 했는데 나중에 교수님께서 변경해달라는 요청이 있었다.
그 부분에 대해서는 다른 게시물에서 이야기할 것이다.

### 💻 작성 코드

```python
# 대각선 폭탄 관련 변수
diagonal_bombs = []  # 대각선 폭탄 정보를 담을 리스트
last_diagonal_bomb_time = 0  # 마지막 대각선 폭탄 생성 시간
diagonal_bomb_interval = random.randint(3000, 5000)  # 3초~5초 사이 간격

(...)

    # 대각선 폭탄 이동 및 화면 갱신
for bomb in diagonal_bombs[:]:
    bomb['rect'].top += bomb['dy']  # 아래로 이동
    bomb['rect'].left += bomb['dx']  # 좌우로 이동
    # 벽에 닿으면 x축 방향 반전
    if bomb['rect'].left <= 0 or bomb['rect'].right >= size[0]:
        bomb['dx'] = -bomb['dx']
    # 화면 밖으로 나가면 삭제
    if bomb['rect'].top > size[1]:
        diagonal_bombs.remove(bomb)
    else:
        screen.blit(bomb_image, bomb['rect'])  # 화면에 그리기

(...)

    # 대각선 폭탄 생성
if elapsed_time - last_diagonal_bomb_time > diagonal_bomb_interval:
    last_diagonal_bomb_time = elapsed_time  # 타이머 초기화
    diagonal_bomb_interval = random.randint(3000, 5000)  # 새 간격 설정
    # 대각선 폭탄 추가
    rect = pygame.Rect(bomb_image.get_rect())
    rect.left = random.randint(0, size[0] - rect.width)  # 화면의 랜덤 위치
    rect.top = -100  # 화면 위에서 시작
    dx = random.choice([-4, 4])  # 왼쪽(-) 또는 오른쪽(+) 방향
    dy = random.randint(5, 10)
    diagonal_bombs.append({'rect': rect, 'dx': dx, 'dy': dy})

(...)

    # 대각선 폭탄 충돌 검사
for bomb in diagonal_bombs[:]:
    offset = (bomb['rect'].left - person.left, bomb['rect'].top - person.top)
    if person_dx == 0:
        collision = check_collision(person_idle_mask, bomb_mask, offset)
    elif person_dx < 0:
        collision = check_collision(person_left_masks[animation_index], bomb_mask, offset)
    else:
        collision = check_collision(person_right_masks[animation_index], bomb_mask, offset)
    if collision:
        diagonal_bombs.remove(bomb)
        # 목숨 감소 로직
        lives -= 1
        if lives <= 0:
            game_over = True
```

이런식으로 코드를 작성했다.

대각선 폭탄 코드를 작성하는 것은 크게 어렵지 않았다.

원래는 `dy`값만 있던 폭탄에 `dx`값을 추가로 가지는 폭탄 로직을 하나 더 생성해서 정해진 시간 사이에 하나씩 나오게 만들면 된다.

> 그런데 이게 뭐람?! 오브젝트들의 충돌 판정이 이상하다?!
> 분명 안 맞은 것 같은데 맞고.. 닿기도 전에 사라지고.. 이딴게 게임??
> 다음 글에서는 오브젝트 판정 완화에 대해 작성할 것이다.
{:.lead}