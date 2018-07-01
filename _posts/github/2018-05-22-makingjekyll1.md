---
layout: post
title:  "지킬 블로그(1)(패키지 설치편)"
date:   2018-05-19
categories: github
comments: true
---

<br/>
## Jekyll 이란?
<br/>
Ruby Gem으로 제공되며 템플릿과 템플릿의 구성요소, 인라인 코드, 마크다운과 같은 동적인 구성요소를 정적인 웹페이지로 만들어주는 파싱 엔진
<br/>

```
** 설치순서 **
루비 설치 > 로컬 블로그 생성 > 로컬로 clone 된 지킬을 확인(locallhost:4000) > push 하여 온라인으로 홈페이지 동기화
```
<br/>
<br/>
## 루비 설치
<br/>
패키지 목록 업데이트
```
sudo apt-get update
sudo apt-get upgrade
```
<br/>
<br/>
Ruby 에 필요한 의존성 패키지 설치
```
sudo apt-get install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev
```
```
zlib1g-dev libncurses5-dev libffi-dev libgdbm-dev
```
<br/><br/>
Ruby 버전관리 프로그램 설치
```
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```
<br/>
<br/>
rbenv 실행 환경변수 설정(zsh 셸)
```
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
```
```
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
```
```
source ~/.zshrc
```
<br/><br/>
Ruby-build 플러그인 설치
```
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
```
<br/><br/>
Ruby 설치하기
```
rbenv install -l
rbenv install 2.5.1
ruby -v
rbenv global 2.5.1
```
