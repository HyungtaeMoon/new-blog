---
layout: post
title:  "dockerfile.base 에는 무엇이 담겨야 하나"
date:   2018-07-17
categories: django
comments: true
---
> 포스팅 내용은 수업자료를 참고하였으며, 개인의 생각이 담겨 있기 때문에 틀린 내용이 담겨있을 수 있습니다.

> 설치 커맨드에 대한 내용은 자동화(Dockerfile.base)를 풀어냈기 때문에 실제 커맨드가 작성하지 않을 수 있습니다.


### # Dockerfile을 생성함에 있어 가장 기본적인 Dockerfile.base 작성하는 방법

`Dockerfile.base`는 **Docker 이미지를 생성**할 때 가장 먼저 사용하는 파일이기 때문에 개발환경에 필요한 패키지들까지 설치를 해주는 것이 좋다.


#### # 우분투를 업데이트 하자

참고로 **우분투**를 설치하지 않는 이유는 aws에서 EC2인스턴스를 생성할 때 1단계(Amazon Machine Image)에서 OS를 선택하고 생성하기 때문에 apt update, apt-dist upgrade 만 업데이트를 진행해주면 된다.

```python
# 우분투 패키지 업데이트
sudo apt update

# 의존성 검사하며 업그레이드(우분투에 각 버전별로 호환성을 따져 최적의 버전끼리 매칭)
sudo apt dist-upgrade
```


#### # 개발환경을 서포팅하는 패키지들을 설치하자

CLI 창에 익숙해져도 오로지 텍스트로만 이루어져있기 때문에 내가 현재 git을 쓰고 있는지, 가상환경에서 하고 있는지 등에 대한 경로등을 확인하는 것이 중요하다. 그래서 bash 보다는 **zsh** 을 설치해서 보다 편한 개발 환경을 만들어보자.

두번째로는 curl 을 사용하는 것이 좋다. 이 **curl**을 사용해서 특정 url 주소에 접근하여 다운로드를 받는다.(cURL은 URL에 client를 붙여서 사용자가 지정한 url로 접근하여 설치를 할 수 있는 커맨드라인 툴이다.

그리고 패키지 작업을 하면서 형상관리도구(git)의 사용은 너무나 당연하다. **git**의 존재에 대한 자세한 내용은 생략한다.

```
sudo apt install zsh curl git
curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh

# 자동화를 할때 필요한 명령어, chsh 명령어는 사용자의 로그인 쉘을 변경
chsh -s /usr/bin/zsh

```

#### # 가상환경을 모르는 자, 개발을 하지 말라

개인이 감당해야 하는 프로젝트는 많고 그 프로젝트의 버전과 패키지들은 각각 다르다. 그래서 탄생했을 것이라고(혼자 추측하는) 가상환경 역시도 꼭 설치를 해야한다.

```python
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash

sudo apt install make build-essential libssl-dev zlib1g-dev libbz2-dev \
                   libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
                   xz-utils tk-dev libffi-dev
```

그리고 가상환경을 사용하기 위해서는 꼭 zshrc에 등록을 해야만 사용이 가능하다.

```
# 경로는 ~/.zshrc
echo 'export PATH="/root/.pyenv/bin:$PATH"'
echo 'eval "$(pyenv init -)"'
echo 'eval "$(pyenv virtualenv-init -)"'
```

#### # 가상환경을 설치하면 그 위에 올려야 하는 첫번째는 사용할 언어(파이썬)이 단연 우선이다

설명이 필요할까..?

```
pyenv install 3.6.5
```

#### # 효율적인 requirements를 위한 pipenv 설치

pyenv는 설치한 패키지들을 친절하게 requirements에서 확인할 수 있지만 내손으로 입력한 패키지 말고도 같이 딸려오는 패키지들까지 리스트들이 쭉 나온다. (그럼 나는 딸려온 수많은 패키지들까지 한꺼번에 설치해야 하는 상황이 오기도 한다)

이를 보완(?)한 것이 pipenv 이고 Pipfile에 각 개발환경(local, dev)환경으로 설치한 패키지들이 나와 한결 수월하다

```
sudo apt install software-properties-common python-software-properties

sudo add-apt-repository ppa:pypa/ppa

sudo apt update

sudo apt install pipenv
```
