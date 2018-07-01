---
layout: post
title:  "지킬 블로그(2)(내 컴퓨터에 적용하기)"
date:   2018-05-19
categories: github
comments: true
---
<br/>
## 지킬 홈페이지 만들기
<br/>
로컬에 블로그 폴더 만들기
```
mkdir blog(블로그명)
> cd blog(블로그명)

jekyll new 블로그명
```
<br/>
<br/>
다른 사람의 git 에서 클론
```
git clone (html형식의 사이트)
```
```
clone 후 해당 폴더로 진입
```
<br/>
가져온 경로를 삭제
```
git remote remove origin
```
<br/>
클론을 재설정
```
git remote add origin (나의 리포 주소)
```
<br/>
경로가 내 리모트로 설정되었는지 확인한다
```
git remote -v origin
```
<br/>
git 에 수정된 내용을 push한다
```
git add -A
git commit -m '메시지'
git push origin master
```
<br/>
내가 생성한 git 경로 안에 있는지 확인
```
ls
```
<br/>
번들 업데이트, 설치를 한다
```
bundle update
bundle install
```
<br/>
config 파일을 수정
```
vi _config.yml
* url, baseurl 의 경로를 삭제
```
<br/>
내 로컬 서버를 열고 로컬 홈페이지에 진입한다
```
bundle exec jekyll serve
```
