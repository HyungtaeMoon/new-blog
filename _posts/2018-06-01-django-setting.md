---
layout: post
title:  "Django 가상환경부터 앱 세팅까지"
date:   2018-06-01
categories: django
comments: true
---
`Django` `환경설정` `수업따라가기`
<br>
<br>
#### # 장고(Django)환경 설정 방법
<br>
<br>
**django 디렉토리 생성**

```
$ mkdir 디렉토리명(생성)

$ cd 디렉토리명(진입)
```
<br>
**git 저장소 생성**

```
$ git init
```
<br>
**.gitignore 생성**

```
https://www.gitignore.io/
gitignore 홈페이지

$ python, django, macos, linux, git, pycharm+all
ignore 추가 항목(6개)

$ vi .gitignore
생성한 텍스트를 .gitignore에 붙여넣기
```
<br>
**장고 설치하기**

```
$ pip install django
최신 버전 설치

$ pip install django~=1.11.0
특정 버전 설치

$ pip list
Django 버전이 설치되었는지 확인
```
<br>
`가상환경 설정`
```
$ pyenv virtualenv 3.6.5 fc-django
가상환경 설치

$ pyenv local fc-django
가상환경으로 진입

(fc-django는 가상환경이름이고 본인이 원하는 이름으로 설정 가능)
```
<br>
`파이참 실행하기`
```
$ charm .

. 의 의미는 현재 경로에서 프로그램을 실행한다는 의미
```
<br>
<br>
`PyCharm Interpreter 설정`
```
파이참 안에서 환경 설정(Ctrl + Alr + S) 실행

[interpreter 경로]
linux ~/.pyenv/versions/fc-django/bin/python


가상 환경(pyenv)을 설정했으면 Existing Environment에 있음
```
<br>
`requirements 기록하기`
```
$ pip freeze > requirements.txt


다른 개발자가 동일한 환경에서 설정해서 구현하도록
requirements.txt는 현재 버전의 정보를 저장
```
<br>
`프로젝트 생성하기(manage.py를 포함)`
```
$ django-admin.py startproject mysite


[프로젝트를 생성하면 아래와 같이 파일이 자동으로 생성]
fc-django(가상환경 설정명)
  mysite
      mysite
        __init__.py
        settings.py
        urls.py
        wsgi.py
    db.sqlite3
    manage.py
  .gitignore
  .python-version
  requirements.txt

manage.py : 컴퓨터에서 웹 서버를 시작할 수 있게 도와줌
settings.y : 웹 사이트 설정이 있는 파일
urls.py : urlresolver가 사용하는 패턴 목록 포함
```
>파일명 변경할 때 주의사항

최상위 `mysite` 는 단순한 폴더
<br>
그래서 파일명을 변경할 때는 **체크박스 2개 모두 해제**
<br>
<br>
바로 아래 `mysite` 폴더는 `패키지`
<br>
Rename할 때는 **체크박스 2개 모두 선택**
<br>
<br>
<br>

`데이터베이스(엑셀 같은) 구조로 반영해주는 migrate`
```
blog의 models.py의 클래스를 정의하고

[변경사항 만들기]
$ python manage.py makemigrations (blog)

Migrations for 'blog':
  blog/migrations/0001_initial.py
    - Create model Post



[변경사항 적용]
$ python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, blog, contenttypes, sessions
Running migrations:
  Applying blog.0001_initial... OK


```
python manage.py migrate 입력 후
<br>
manage.py와 동일한 위치에 db.sqlite3(장고의 기본 데이터 베이스 어댑터)가 생성되었는지 확인
<br>
<br>
데이터베이스 안의 모델은 엑셀의 스프레드시트라고 생각하자.
<br>
<br>
이로써 settings.py 의 INSTALLED_APPS 변경사항 내용 적용되었고, 하나의 테이블이 생겼다.
<br>
<br>
<br>
`$ python manage.py migrate를 하면`
```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying sessions.0001_initial... OK

```
<br>
<br>

--------------------------------
<br>
<br>
### # `폴더 경로 지정하기`
<br>
**폴더의 구조**
<br>
절대적인 경로는 아니고 이러한 방식으로 폴더가 구성된다 정도로 익혀두자.
```
djangogirls-tutorial/ <- 이 프로젝트의 컨테이너 폴더(Root폴더)
  app/  <- Django프로젝트 관련 코드 컨테이너 폴더
    config/  <- Django프로젝트의 설정 관련 패키지
      settings.py
  .gitignore  <- 프로젝트를 git하기 위해 필요한 파일들
  .git/
  requirements.txt
  ...
```
<br>
<br>
`경로 이해하기`
<br>
**PyCharm은 현재 프로젝트 구조에서 가장 상위폴더를 파이썬 Source의 root로 인식한다**
```
project/ <- python 실행
  project_module1.py
  project_module2.py
  pac1/
    __init__.py
    pac1_module.py
    pac2/
      __init__.py
      pac2_module.py
      pac3/
        __init__.py
```
<br>
`Q. project_module1.py 를 가져오고 싶다면?`
<br>
import project_module1
<br>
<br>
<br>
`Q. pac1 패키지 안의 pac1_module.py를 가져오고 싶다면?`
from import project_module1
<br>
<br>
<br>
`Q. pac2 패키지 안의 pac2_module를 가져오고 싶다면?`
<br>
from pac1.pac2 import pac2.pac1_module
<br>
<br>
<br>
`python을 pac1에서 시작했을 때 project_module를 가져오고 싶다면?`
루트폴더 하위에서만 작업이 가능하기 때문에 불가능하다.
<br>
<br>
<br>
`패키지와 경로 설정 지정 방법은 다르다.`
<br>
알고 있던 경로 접근 방식은 `/` 였는데 패키지는 `.` 으로 접근을 한다. 왜냐하면 패키지가 가진 모듈은 `패키지의 속성처럼 접근`을 하기 때문이다.
<br>
<br>
폴더를 생성할 때 `Directory`와 `Python Package`의 차이점
<br>
`Python Package`는 생성할 때 '__init__.py'파일을 같이 생성해준다.
<br>
<br>
### # 자주 사용하게 될 명령어 모음
<br>
`loopback` 주소
<br>
```
127.0.0.1:8000

127.0.0.1:4000

...
```
<br>
이 주소로 ip를 보내면 밖으로 안 나가고 다시 나에게 돌아오는 주소.
<br>굳이 다른 서버로 보내고 하는 번거로운 작업들이 필요 없을 때 이 'loopback'을 사용한다.
<br>
<br>
`로컬 서버 가동하기`
```
manage.py가 있는 폴더에서

$ python manage.py runserver

또는

$ ./manage.py runserver
```
<br>
<br>
`프로젝트 시작하기`
```
django-admin startproject mysite .
```
<br>
<br>
`블로그 생성하기`
```
$ python manage.py startapp blog .

생성한 후에는 settings.py에 'blog' 추가
```
<br>
<br>
자주 사용하는 Completion/Basic(자동완성)
<br>
`환경설정` - `Keymap` - `Ctrl + Space`
<br>
<br>
<br>
`장고 관리자 페이지 만들기`
```python
파일명: app/blog/admin.py

from django.contrib import admin

from .models import Post

admin.site.register(post)
```
<br>
`관리자 페이지 접근 방법(admin 생성)`
<br>
```
python manage.py createsuperuser
```
<br>
<br>
admin 페이지에 아래 테이블은 내가 뭘 해서 생겼지?
```
blog
Post    +Add &change
```
blog/models.py에서 `Post`클래스를 정의
<br>
blog/admin.py에서 `Post`클래스를 등록
<br>
<br>
`장고 쉘 실행하기`
```
$ python manage.py shell
```
<br>
<br>
`blog/models 에 있는 글 불러오기`
```
$ from blog.models import Post
$ Post.objects.all()

<QuerySet [<Post: 무디스 "韓 통신비 인하 정책, 이통사 신용도 떨어진다">, <Post: 삼성전자 '애플에 5800억원 배상' 美 평결에 재심 청구>, <Post: "종이 증명 시대 끝…디지털트윈 실현될 것">, <Post: [종합]이통3사, 지방선거·월드컵 대비 '이상무'>, <Post: [고든 정의 TECH+] 슈퍼컴퓨터 왕좌 되찾은 미국…앞으로의 과제는?>]>

쿼리셋 형식으로 127.0.0.1:8000에서

내가 작성한 글들이 출력된다.(성공적)
```
