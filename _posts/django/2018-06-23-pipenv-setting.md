---
layout: post
title:  "pipenv 설정"
date:   2018-06-26
categories: django
comments: true
---
`pipenv설정`
<br>
[settings.py]
```
django_extentions
```
<br>
**migrations (일부))초기화**
```
./manage.py showmigrations

./manage.py migrate proxy(zero)
```
<br>
<br>
**pipenv 설치**
```
sudo apt install python3-pipenv
pip3 install --upgrade pip

pip install --user pipenv
```
<br>
<br>
```
vim ~/.zshrc

# If you come from bash you might have to change your $PATH.
export PATH=$HOME/bin:/usr/local/bin:$PATH
export PATH=$HOME/.local/bin:$PATH <추가>
```
<br>
<br>
### # pipenv 설치
<br>
<br>
```
pipenv --python 3.6.5

vi Pipfile(버전 확인)

pipenv install django에

pipenv shell을
django-admin.py startproject configuration

[가상환경 경로]
home/shape/.virtualenvs/instagram-5vHaF3RF/bin/python

pip install pillow
```
