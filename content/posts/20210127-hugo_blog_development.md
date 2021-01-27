---
title: "Hugo+Github Page 정적 웹페이지 블로그 만들기"
date: 2021-01-27T12:39:08+09:00
Categories = ["Notes"]
tags: ["Git", "Blog", "Hugo"]
---

### 휴고를 선택한 이유

블로그 빌딩에 Hugo와 Jekyll을 두고 고민했다.
Jekyll이 더 널리 사용되는 것은 많으나 블로그 빌딩에 많은 시간이 소요된다고 한다. 수 천개의 토막글이 올라갈 공간이기에 빌딩 타임은 중요하다.

![hugo_vs_jekyll_building_time_comparison](https://res.cloudinary.com/forestry-io/image/fetch/c_limit,dpr_auto,f_auto,q_80,w_1382/https://forestry.io/uploads/2018/01/hugo-vs-jekyll-basic-test-1.png)

### 휴고 블로그 만들기

다른 사람들의 휴고 블로그 튜토리얼을 보고 설치를 시작했다.

#### 휴고 설치

맥을 사용하기에 hombrew로 hugo를 설치한다.
이어 Hugo new site <프로젝트 이름> 한 줄로 블로그 디렉토리를 만들 수 있다.

```
Brew install hugo
Hugo new site <프로젝트 이름>
```

#### 테마 설치

[휴고 테마 사이트](https://themes.gohugo.io)에서 원하는 테마를 선택한 뒤 github repository의 주소를 복사한다.
프로젝트 디렉토리에서 submodule로 추가해준다.
서브모듈로 사용하는 이유는 잠시 후에 다뤄보자.
마지막으로 config.toml 파일에서 테마명을 지정해줘야 한다.

```bash
git init
git submodule add <anubis.git> themes/anubis
echo 'theme = "anubis"' >> config.toml
```

#### 테스트 블로그 빌딩

블로그 빌딩을 위한 마크다운 파일을 만들어준다.
Hugo server -D 커맨드를 입력하면 로컬 서버에서 블로그를 빌딩한다.
포트 번호는 1313이며 -D 옵션을 사용하면 포스트가 draft=true라도 빌딩에 올린다.
주소창에 "localhost:1313"를 넣으면 블로그를 확인할 수 있다.

```
hugo new posts/test.md
hugo server -D
```

#### 깃헙 저장소에 연동

깃헙에 두 개의 repo를 만든다. 

1. hugo_blog_repo
2. junuMoon.github.io

1번은 실제 데이터가 올라가는 repo다.
맘대로 이름 짓는다.
로컬 저장소와 github 저장소를 연동한다.

```bash
git remote add origin <hugo_blog_repo.git>
git push origin master
```

2번은 깃헙 페이지 사이트 디렉토리다.
따라서 github 사용자명에 .github.io를 붙여서 만들어야 한다.

> 2번 저장소를 생성할 때 README.md 옵션을 클릭해서 만들도록 하자. 이 저장소는 서브모듈로 추가할 저장소인데 비어있을 시 에러가 발생한다.

아까 블로그 빌딩에서 public이란 디렉토리가 생성됐다.
이 디렉토리가 웹사이트 디렉토리다.
GitHub 페이지를 사용할 것이기 때문에 원래 있던 public은 지워준다.
대신 깃헙 저장소를 서브모듈로 추가하고 public이란 이름을 준다.

```bash
rm -rf public
git submodule add -b main <junuMoon.github.io.git> public
```

#### 포스팅

이제 포스팅을 해보자. 'hugo' 명령어로 사이트를 빌드할 수 있다.
간편하다.

```bash
hugo
```

이렇게 하면 public 디렉토리에 사이트가 빌딩된다.
이를 github 저장소에 업데이트 하자.

```bash
cd public
git add .
git commit -m "first blog building"
git push origin master
```

실제 데이터가 들어갈 hugo_blog_repo도 업데이트 해주지 않으면 포스트가 블로그에 올라가지 않는다.

```bash
cd ..
git add .
git commit -m "post update: 20210127-test"
git push origin master
```

자신의 주소로 들어가면 정상작동하는 것을 볼 수 있다.

### 오류

#### 1. 'origin/master' is not a commit and a branch 'master' cannot be created from it

```bash
$ git submodule add -b master <junumoon.github.io> public
warning: You appear to have cloned an empty repository.
fatal: 'origin/master' is not a commit and a branch 'master' cannot be created from it
Unable to checkout submodule 'public'
```

난 오류만 나오면 치가 떨리고 손이 흐른다.
구글 번역기를 돌리자.

```bash
경고 : 빈 저장소를 복제 한 것 같습니다.
치명적 : 'origin / master'는 커밋이 아니며 'master' branch를 만들 수 없습니다.
서브모듈 'public'을 체크아웃 할 수 없습니다.
```

빈 저장소를 서브모듈로 추가하려 해서 생기는 문제다.
github 저장소를 만들 때 reame.md 파일 추가 옵션을 설정하면 이런 오류를 피할 수 있다.
그렇게 저장소 만들면 처음 생기는 branch가 main이니까 다음 처럼 서브모듈 추가 커맨드를 주면 된다.

```bash
$ git submodule add -b main <junumoon.github.io> public
```

### Hugo 블로그를 사용하는 이유

git을 이용한 글쓰기 프로세스를 만드는 중이다.
Writing-project라는 git repo에서 git을 이용해 글을 쌓아올린다.
하나의 글 프로젝트로서 발전시키기 전에 짧은 글, 자투리 생각, 소재 등을 모아놓는 디렉토리가 필요했다.
github page로 블로그를 만들어서 사용하기로 결정했다.
여기에는 형식과 주제에 제한 없이 자유롭게 올릴 계획이다.

