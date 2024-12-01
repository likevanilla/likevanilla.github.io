---
layout: post
title: "Pygame 프로젝트 개발기 (2)"
description: 처음 해보는 github 협업 프로젝트.. 잘 해낼 수 있을까??

categories: python
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}



* toc
{:toc}



## 들어가기 전....
> Github에 있는 프로젝트 레포지토리를 링크로 달고싶은데 교수님께서 비공개?로 해두셨다,,
> 내 프로젝트 다른 사람들한테 보여주고싶은데~ 진짜 잘 했는데~
{:.lead}


## 🛠 구현 기능
초반 회의 때 어떤 기능들을 추가할 지 이야기를 나눴다.

- 시작 화면
![시작 화면](/_posts/python/pygame1/start.png){:.lead width="200" height="400" loading="lazy"}

<hr>

- 플레이 화면
![플레이 화면](https://private-user-images.githubusercontent.com/87215294/371839176-44ed3641-2a18-4270-aac0-c86732d7d556.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMwNDg0MzIsIm5iZiI6MTczMzA0ODEzMiwicGF0aCI6Ii84NzIxNTI5NC8zNzE4MzkxNzYtNDRlZDM2NDEtMmExOC00MjcwLWFhYzAtYzg2NzMyZDdkNTU2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMDElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjAxVDEwMTUzMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTMxMDU0ZWI1MDhmODVhNDVkZTNlYmFmYTMxYTI5YWFmOTk5NDRkZmEzNjJlZGYwOWMyOWU3YTY5M2ZjM2UwZjgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.GxomhnUIdXoVCo2vq4_e20Q9rThsmgHhMWV5c1Z7oSk){:.lead width="100%" height="auto" loading="lazy"}

<hr>

- 종료 화면
![종료 화면](https://private-user-images.githubusercontent.com/87215294/371839176-44ed3641-2a18-4270-aac0-c86732d7d556.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzMwNDg0MzIsIm5iZiI6MTczMzA0ODEzMiwicGF0aCI6Ii84NzIxNTI5NC8zNzE4MzkxNzYtNDRlZDM2NDEtMmExOC00MjcwLWFhYzAtYzg2NzMyZDdkNTU2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMDElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjAxVDEwMTUzMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTMxMDU0ZWI1MDhmODVhNDVkZTNlYmFmYTMxYTI5YWFmOTk5NDRkZmEzNjJlZGYwOWMyOWU3YTY5M2ZjM2UwZjgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.GxomhnUIdXoVCo2vq4_e20Q9rThsmgHhMWV5c1Z7oSk)



그렇다.. 그림판으로 대충 틀을 잡아본 그림이다... 참고로 초기에는 쓰레기를 피하는 게임이었던 것
{:.faded}


먼저 화면 구성은 이렇게 하기로 했다. 그렇다면 원본 게임에서 어떤 것들을 추가해야 저렇게 보일 수 있을까?


## 🧠 생각한 대로 이루어지리.. 
1. 시작 화면, 플레이 화면, 종료 화면 총 3가지로 상황에 맞는 화면 나누기
2. 시작 화면에 START 버튼, 로고 만들기
3. 플레이 화면에 시간 나오게 하기
4. 생명 시스템 구현하고 표시하기
5. 종료 화면에 재시작, 게임 종료, 종료되었을 때의 시간이 나오게 하기
6. 게임오버가 되면 위에서 떨어지던 오브젝트들과 캐릭터 사라지게 하기

위 사진들만 보면 대충 이정도가 떠올랐다.

여기서 세세하게 기능들을 나누고 팀원들과 하나씩 나눠서 개발을 하기로 했다.

나의 첫 임무는 종료 화면 만들기..!


## 종료 화면 만들기
처음 생각해낸 방법은 아직 생명 시스템이 구현이 되어있지 않기때문에 그저 폭탄과 캐릭터가 충돌하면 종료화면으로 넘어가게 하면 된다.

알고리즘은 아래와 같다.

1. 폭탄과 캐릭터가 충돌한다.
2. 만들어둔 game_over 변수를 True로 변경한다.
3. if game_over: 조건문을 만들어서 game_over가 True가 되면 작동하게 만든다.
4. 조건문의 내용으로는 `Game Over`를 표시하고 게임 화면을 업데이트해서 오브젝트들을 다 없앤다.

추가로 게임 오버가 됐을 때는 폭탄이 더이상 생성되지 못하게 폭탄 생성 코드에 not game_over 조건도 추가해줬다.

변경된 코드는 아래와 같다.


```Python
import pygame  # 1. pygame 선언
import random
import os

pygame.init()  # 2. pygame 초기화

# 3. pygame에 사용되는 전역변수 선언

BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
size = [600, 800]
screen = pygame.display.set_mode(size)

done = False
clock = pygame.time.Clock()

game_over = False  # 게임 오버 상태를 나타내는 변수

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

    global done, game_over
    font = pygame.font.SysFont(None, 75)  # 게임오버 텍스트를 위한 폰트 설정

    while not done:
        clock.tick(30)
        screen.fill(BLACK)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                done = True
                break
            elif event.type == pygame.KEYDOWN and not game_over:
                if event.key == pygame.K_LEFT:
                    person_dx = -5
                elif event.key == pygame.K_RIGHT:
                    person_dx = 5
            elif event.type == pygame.KEYUP:
                if event.key == pygame.K_LEFT:
                    person_dx = 0
                elif event.key == pygame.K_RIGHT:
                    person_dx = 0

        if not game_over:
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
                    game_over = True
                screen.blit(bomb_image, bomb['rect'])

        if game_over:
            game_over_text = font.render("Game Over", True, WHITE)
            screen.blit(game_over_text, (size[0] // 2 - game_over_text.get_width() // 2, size[1] // 2 - game_over_text.get_height() // 2))
            pygame.display.update()

        pygame.display.update()

runGame()
pygame.quit()
```

- 결과물
![게임 오버](_posts\python\pygame1\game_over.png){:.lead width="200" height="400" loading="lazy"}

그렇다.. 정말 딱 기초적인 부분만 구현을 한 것이다.

제일 큰 문제점은 이 화면에서 x를 클릭해서 게임을 끄는 것 말고는 할 수 있는 게 없다는 것..!!

당연히 점점 개발을 하면서 고쳐나갈 부분이다.

그래도 내가 뭔가를 해서 올렸다는 게 뿌듯하고 신기했다ㅎㅎ



> 다음 글은 라이선스에 대해 작성할 예정이다. 왜냐하면 그 다음 나의 임무가 라이선스 생성이기 때문!
{:.lead}