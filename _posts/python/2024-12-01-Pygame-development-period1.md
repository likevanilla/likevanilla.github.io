---
layout: post
title: "Pygame 프로젝트 개발기 - 시작"
description: 처음 해보는 github 협업 프로젝트.. 잘 해낼 수 있을까??

categories: python
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}



* toc
{:toc}



## 들어가기 전....
> 이번에 대학교에서 듣는 "오픈소스SW"라는 수업에서 Github와 Pygame으로 팀 프로젝트를 하게되었다.
> 처음으로 해보는 프로젝트라서 떨리기도 하고.. 잘 할 수 있을까 싶기도 하고..
> git도 수업 때 배웠지만 모르는 부분도 많아서 실수도 많이 했지만 하다보니 기본적인 부분들은 익숙해졌당ㅎ~ㅎ
{:.lead}


# 🎮 어떤 게임을 만들까?
팀이 정해지고 가장 먼저 떠오른 생각은 당연히 "어떤 게임을 만들까?"였다.

구글에 Pygame을 구글링하며 결정된 것은 바로... **폭탄 피하기!!**

이 게임을 만드는 설명 블로그도 잘 되어있고 코드도 제공을 하셔서 기본 코드로 이용하기로 했다.

[참고한 블로그](https://ai-creator.tistory.com/529)



```python

import pygame  # 1. pygame 선언
import random
import os

pygame.init()  # 2. pygame 초기화

# 3. pygame에 사용되는 전역변수 선언

BLACK = (0, 0, 0)
size = [600, 800]
screen = pygame.display.set_mode(size)

done = False
clock = pygame.time.Clock()

def runGame():
    bomb_image = pygame.image.load('bomb.png')
    bomb_image = pygame.transform.scale(bomb_image, (50, 50))
    bombs = []

    for i in range(5):
        rect = pygame.Rect(bomb_image.get_rect())
        rect.left = random.randint(0, size[0])
        rect.top = -100
        dy = random.randint(3, 9)
        bombs.append({'rect': rect, 'dy': dy})

    person_image = pygame.image.load('person.png')
    person_image = pygame.transform.scale(person_image, (100, 100))
    person = pygame.Rect(person_image.get_rect())
    person.left = size[0] // 2 - person.width // 2
    person.top = size[1] - person.height
    person_dx = 0
    person_dy = 0

    global done
    while not done:
        clock.tick(30)
        screen.fill(BLACK)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                done = True
                break
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    person_dx = -5
                elif event.key == pygame.K_RIGHT:
                    person_dx = 5
            elif event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT:
                    person_dx = 0
                elif event.key == pygame.K_RIGHT:
                    person_dx = 0

        for bomb in bombs:
            bomb['rect'].top += bomb['dy']
            if bomb['rect'].top > size[1]:
                bombs.remove(bomb)
                rect = pygame.Rect(bomb_image.get_rect())
                rect.left = random.randint(0, size[0])
                rect.top = -100
                dy = random.randint(3, 9)
                bombs.append({'rect': rect, 'dy': dy})

        person.left = person.left + person_dx

        if person.left < 0:
            person.left = 0
        elif person.left > size[0] - person.width:
            person.left = size[0] - person.width

        screen.blit(person_image, person)

        for bomb in bombs:
            if bomb['rect'].colliderect(person):
                done = True
            screen.blit(bomb_image, bomb['rect'])

        pygame.display.update()


runGame()
pygame.quit()
```

![게임 플레이](https://github.com/likevanilla/likevanilla.github.io/blob/main/_posts/python/pygame1/playing.png?raw=true){:.lead width="200" height="400" loading="lazy"}
<br>
게임 플레이 화면임당
{:.figcaption}



# 코드 이해하기
아무래도 Pygame은 처음인지라 코드부터 이해하기로 했다.

주석이 어느정도 달려있어서 이해하기 어렵지 않았지만 다른 팀원들도 이해할 수 있게 하나하나 직접 이해하고 주석을 달아보았다.
~~주석만 추가했을 뿐이지만 살짝 난잡해 보였던 건 안 비밀~~

추가로 여러 기능들을 개발하면서 게임을 **업그레이드**를 해야하기 때문에 pygame에 대한 공부도 필요했다.

[참고한 강의](https://www.youtube.com/watch?v=j4J6m81ccto&list=PL1jdJcP6uQtudj1sjGUZNA_4TkgJaYKC3)

# 👥 우당탕탕 Git 협업
다들 Git 협업은 처음 해봐서 첫 회의 때는 Git을 어떻게 활용하고 어떤 규칙으로 소통할 것인지 공부하고 정했다.

사실 아직도 제대로 할 수 있는 게 아니라서 설명을 잘 할 자신은 없다... 더 공부한 내가 나중에 보고 웃음이 나온다면 오케이..

생각나는 것들을 적어보면...
- Issue와 PR 템플릿을 만들어서 구성과 내용에 맞게 작성 후 올리기
- Dev 브랜치를 따로 만들고 거기서 기능 개발을 할 때는 관련된 새로운 브랜치를 또 뻗어서 개발 후 Dev랑 병합하기
- Commit 메세지 형식 맞추기
- PR 올리면 단톡방에 올려서 다른 사람들에게 알리기

교수님 : "나중에 실무로 가면 GitHub로 소통하지 따로 단톡방에서 소통하진 않아요"
{:.faded}

~~프로젝트가 끝날 때 쯤에 한 말씀이시다.. 그치만 이미 다 끝나가는걸요~~~

Git 통일은 이정도였던 것 같다.

그 뒤로는 어떤 기능들을 개발할 것인지 여러가지 아이디어들을 꺼내놓으며 회의를 이어나갔다.