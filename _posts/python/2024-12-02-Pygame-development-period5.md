---
layout: post
title: "Pygame 프로젝트 개발기 - 배경 음악(BGM) 추가"
description: 게임을 하는데 BGM은 무조건 있어줘야지!

categories: python
permalink: '/:categories/:year/:month/:day/:title'

published: true
---
{% include hits.md %}


* toc
{:toc}


## 들어가기 전....
> 게임을 하고있는데 허전한 느낌이 든다.. 음악이 없다 음악이!!!
> 게임의 필수 요소인 BGM 기능을 추가해보자!
{:.lead}

## 🎵 어떤 음악으로 고를까
정해진 컨셉인 픽셀 아트와 관련된 음악으로 골라야한다.

그래서 8비트 음악으로 선정했다.

[배경음악(BGM) 출처](https://opengameart.org/content/4-chiptunes-adventure)

4가지 음악 중에서 Stage1, Stage2 두 개를 선정했다.

음악이 흥미를 돋구고 긴장감을 조성해서 게임의 장르에 잘 맞는 느낌이다.

~~아무리 들어도 잘 고른 느낌이 든다ㅎㅎ~~
{:.faded}

음악은 링크를 타고 들어가서 들어야될 것 같다. 블로그에 어떻게 올리는 지 모름..
그리고 소리가 클 수 있으니 미리 조절을 하고 듣는 것을 추천!!!!!!!!!!!
{:.note}

## 적용하자~

처음에는 그냥 이런식으로 BGM을 추가했다.

```python
pygame.init()  # pygame 초기화
pygame.mixer.init() # pygame mixer 초기화 (배경 음악)
# 배경 음악 파일 로드
bgm_1 = 'bomb_game/sound/BGM1.wav'
bgm_2 = 'bomb_game/sound/BGM2.wav'

# 배경 음악 소리 조절
pygame.mixer.music.set_volume(0.3)

def runGame(): 
    pygame.mixer.music.load(bgm_1)  # 첫 번째 음악을 로드
    pygame.mixer.music.queue(bgm_2) # 두 번째 음악을 대기열에 추가
    pygame.mixer.music.play()  # 첫 번째 음악 재생

```

잘 나왔다!

그런데......

## 🔈 이게 무슨 일이람??
어느날 학교에서 코드를 실행하는 순간... pygame.mixer()에서 오류가 발생했다!!!

비~~~~~~~~상~

오류의 내용을 해석해보니 컴퓨터에 연결된 재생 장치가 발견되지 않으면 발생하는 문제라고 한다.

잠시 고민을 하다가 떠오른 해결방법!

pygame.mixer() 부분을 try-except 예외처리 문법으로 해결하기 !

내가 이걸 여기서 쓸 줄은 상상도 못했네..ㄴㅇㄱ
{:.faded}

## Try-except 예외처리

```python
try:
    pygame.mixer.init() # pygame mixer 초기화 (배경 음악)
    pygame.mixer.music.set_volume(0.3)
except pygame.error as e:
    print("재생 장치 관련 오류로 인해 BGM이 나오지 않습니다.")
    print(f"{e}")

try:
    pygame.mixer.music.load(bgm_1)  # 첫 번째 음악을 로드
    pygame.mixer.music.queue(bgm_2) # 두 번째 음악을 대기열에 추가
    pygame.mixer.music.play()  # 첫 번째 음악 재생
except pygame.error as e:
    print("재생 장치 관련 오류로 인해 BGM이 나오지 않습니다.")
    print(f"{e}")
```

이런식으로 바꿔주니 잘 작동됐다~ 휴~ 

HAPPY ENDING🎆🎉✨
{:.lead}