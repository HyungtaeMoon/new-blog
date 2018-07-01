---
layout: post
title:  "Virtualenv 설치하기"
date:   2018-06-22
categories: django
comments: true
---
`virtualenv` `설치는 하는데 이게 뭔지`
<br>
<br>
**모든 포스팅은 수업내용 및 혼자 알아본 내용으로 사실과 다를 수도 있습니다.**
<br>
<br>
#### # Virtualenv??
<br>
>`Virtualenv`는 `Virtual environment(가상환경)`의 줄임말

<br>
<br>
실제로 프로젝트를 여러개 진행하다보면 파이썬의 버전을 단 한개의 버전으로만 작업을 할 수가 없다.
<br>
<br>
그래서 프로젝트를 시작 하기 전에 프로젝트 공간에 `작업을 진행할 파이썬 버전`을 단독적으로 설치해서, 다른 라이브러리(다른 작업 공간)와 겹치지 않게 작업하기 위함이다.
<br>
<br>
(새로운 프로젝트를 할 때마다 pyenv virtual `3.6.5` 의 의미가 `가상환경은 파이썬의 3.6.5 버전으로 설정하겠다`라는 것을 이번에 정리하며 알게 되었다.)
<br>
<br>
가상 환경을 설정하기 전에 virtualenv 가 설치되었는지 확인하는 작업이 필요하다.
```
$ virtualenv --version
```
**가상환경을 설정하고 그 안에서 명령여를 쳐봤는데 `zsh: command not found: virtualenv
`메시지가 뜬다. 일단 넘어가자**
<br>
<br>
`virtualenv 설치방법`
```
$ sudo pip install virtualenv
```
**최초 1번만 설치해두면 된다.**
<br>
<br>
#### # pyenv(가상환경) 생성
`pyenv virtual 3.6.5 study(가상환경명)`
```
Requirement already satisfied: setuptools in /home/shape/.pyenv/versions/3.6.5/envs/study/lib/python3.6/site-packages
Requirement already satisfied: pip in /home/shape/.pyenv/versions/3.6.5/envs/study/lib/python3.6/site-packages

```
python3.6/site packages의 요구사항이 만족되었단다.
<br>
<br>
#### # pyenv(가상환경) 적용
<br>
```
$ pyenv local study(가상환경명)

(study) ➜  study git:(master) ✗
(가상환경명)  ➜  study git:(master) ✗
```
가상환경 설정이 완료되었다.
<br>
<br>
#### # 번외
<br>
`파이썬 버전 알아보기`
```
$ python --versions

Python 3.6.5

```
