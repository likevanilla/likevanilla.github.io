---
layout: post
title: "[Github] Github.io Blog 만들기"
description: Github에서 Bolg를 만들고 싶을 때는 어떻게 하면 될까?

categories: gitblog
permalink: "/:categories/:year/:month/:day/:title"

published: true
---

{% include hits.md %}

- toc
  {:toc}

## WARNING!

> 이 글은 chirpy 테마를 기준으로 작성된 글이며 현재는 Hydejack 테마로 바꾼 상태라 원하시는 정보와 다를 수 있습니다!
> {:.lead}

## Windows에서 Github Blog 만들기 (github.io)

## Github Blog란?

Github Pages라고도 불리며 Github를 통해 호스트되고 게시되는 퍼블릭 웹 페이지입니다.

Github Pages는 개발자분들이 블로그, 개인 프로젝트 전시용, 포트톨리오 등의 목적으로 많이 사용합니다.

&nbsp;
&nbsp;

## 시작하기 전에

- 운영체제는 `Windows 64blt`를 기준으로 설명을 합니다.
- [Github](https://github.com/signup?source=login) 회원가입
- [Visual Studio Code](https://code.visualstudio.com/download), [Git](https://git-scm.com/downloads) 설치
- [Ruby](https://rubyinstaller.org/downloads/) (x64) 설치
- [Node.js](https://nodejs.org/en/) 설치
- [chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy) Download zip (선택)

&nbsp;
&nbsp;

## 새로운 저장소(Repository) 만들기

1. 자신의 Repositories에 들어가서 `New`버튼을 눌러준다.
2. Repository name은 user_name.github.io로 생성한다.
   ex) likevanilla.github.io
3. Public, Add a README file을 선택해준다.
4. `Create repository`

&nbsp;
&nbsp;

### ⚙️ Settings

1. user_name/github.io 저장소(repository)로 이동
2. Settings - Pages - Source - Deploy form a branch 선택
3. https://user_name.github.io 접속 확인

### ❌ 접속이 되지 않는 경우

1. Settings - Pages - Branch를 `main`으로 변경
2. https://user_name/github.io

&nbsp;
&nbsp;

### Visual Studio Code를 사용하여 로컬에서 작업하기

1. Start `Visual Stdio code`
2. Press `F1`
3. Search `git clone`
4. Click `Git:Clone`
5. Copy and paste Repository URL
6. Choose file to clone

### ❗ 주의사항

- 저장된 곳으로 들어가는 경로의 파일들 이름은 영어로 설정해야합니다.
  추후 관련 오류 코드 - 3221225477

&nbsp;
&nbsp;

### 로컬 변경사항 적용

1. 클론한 Repository 파일 열기
2. `index.html` 파일 생성 후 아래 내용 작성

```html
<html>
  <body>
    Hello! This is the first page!
  </body>
</html>
```

3. 좌측 `Sorce Control(소스 제어)` 선택
4. `+` 버튼을 클릭하여 변경 사항 추가
5. 커밋 메세지 입력, 커밋 & 푸시
6. https://user_name.github.io 또는 http://user_name/github.io 확인

&nbsp;
&nbsp;

## 로컬 개발 환경 구축

- 윈도우 키 또는 시작 버튼을 누르고 `Start Command Prompt with Ruby` 관리자 권한으로 실행
- Repository가 있는 위치로 이동

```bash
# 예시
cd C:\Users\wjdgu\Documents\blog\likevanilla.github.io
# 또는
cd C:/Users/wjdgu/Documents/blog/likevanilla.github.io
```

- `jekyll, bundler, webrick` 설치

```bash
gem install jekyll bundler
gem install webrick
```

- 설치 확인

```bash
ruby -v
jekyll -v
bundler -v
```

&nbsp;
&nbsp;

### Jekyll 서버 구축

- `jekyll` 생성

```bash
jekyll new ./
# 또는
jekyll new ./ --force
```

- bundle install

```bash
bundle install
```

- jekyll 서버 실행

```bash
bundle exec jekyll serve
```

- 접속 확인
  a. [https://127.0.0.1:4000/](https://127.0.0.1:4000/) 또는 [https://localhost:4000/](https://localhost:4000/)
  b. [http://127.0.0.1:4000/](https://127.0.0.1:4000/) 또는 [http://localhost:4000/](https://localhost:4000/)

&nbsp;
&nbsp;

## Jekyll 테마 적용

### 테마 선택

- 위에서 `chirpy theme`을 설치하지 않았다면 아래 사이트 중에서 원하는 theme을 골라서 설치하세요. 설명은 `chirpy theme`을 설치했다는 기준으로 진행합니다.
  - [http://jekyllthemes.org](http://jekyllthemes.org/)
  - [https://jekyllthemes.io/free](https://jekyllthemes.io/free)
  - [https://themes.jekyllrc.org](https://themes.jekyllrc.org/)
  - [https://github.com/topics/jekyll-theme](https://github.com/topics/jekyll-theme)

1. `jekyll-theme-cirpy-master.zip` 압축을 풀고 모든 내용을 user_name.github.io 파일 안에 복사(덮어쓰기)
2. bundle install

```bash
bundle install
```

3. jekyll 서버 실행

```bash
bundle exec jekyll serve
```

4. 접속 확인
   a. [https://127.0.0.1:4000/](https://127.0.0.1:4000/)또는 [https://localhost:4000/](https://localhost:4000/)

   b. [http://127.0.0.1:4000/](https://127.0.0.1:4000/) 또는 [http://localhost:4000/](https://localhost:4000/)

5. 좌측 `Sorce Control(소스 제어)` 선택
6. `+` 버튼을 클릭하여 변경 사항 추가
7. 커밋 메세지 입력, 커밋 & 푸시

&nbsp;
&nbsp;

## Github Action 적용

1. user_name/github.io 저장소(repository)로 이동
2. Settings - Pages - Source - `Github Actions` 선택
3. `Configure` 클릭
4. `Commit changes…` 클릭
5. Visual Studio Code에서 Pull 또는 git clone 과정 다시하기

&nbsp;
&nbsp;

## Node.js 설치

```bash
npm install && npm run build
```

### ❌ 에러가 뜰 때

1. `npm error code 3221225477`
   - 해결 방법 : 프로젝트를 영문 경로로 이동
2. `npm error code ENOENT`

   - 해결 방법 : pakage.json 파일을 만들거나 들어가서 아래의 코드를 작성한 뒤 설치

   ```json
   {
     "name": "my-github-blog",
     "version": "1.0.0",
     "scripts": { "build": "jekyll build", "serve": "jekyll serve" }
   }
   ```

&nbsp;
&nbsp;

## Theme 상세 설정

- `.gitignore` 파일 수정

```
# Misc
# _sass/dist
# assets/js/dist
```

- `_config.yml` 파일 수정
  - 추가적으로 다른 부분의 정보들을 바꿔도 됨(github, twitter, social …)

```
# 12번째 줄
timezone: Asia/Seoul

# 26번째 줄
url: "https://karina.github.io"

# 28번째 줄
github:
  username: karina
```

1. 좌측 `Sorce Control(소스 제어)` 선택
2. `+` 버튼을 클릭하여 변경 사항 추
3. 커밋 메세지 입력, 커밋 & 푸시
4. https://user_name.github.io 또는 http://user_name/github.io 확인
