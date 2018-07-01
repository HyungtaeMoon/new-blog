---
layout: post
title:  "Django에서 경로설정 확인하기"
date:   2018-06-22
categories: django
comments: true
---
`장고` `홈페이지`
<br>
<br>
#### # 장고 설치 방법
<br>
```
pip install django~=1.11.0

```
<br>
<br>
#### # 누군가가 서버에 웹 사이트를 요청하면!?

<br>
<br>
1) 사용자가 `Browser`를 쓰고
<br>
2) (`request`)요청을 한다.
<br>
3) 어디에? `Server`에
<br>
4) 그럼 서버는 `Django application` 실행
<br>
5) `urlresolver`가 아래 중에서 어디로 갈지 경로 설정
<br>
5-1)
<br>
view1(선택) -> (process) -> (response)
<br>
view2
<br>
view3
<br>
6) Server
<br>
7) Browser
<br>
<br>
#### # runserver 실행 방법
<br>
```
[manage.py가 있는 폴더에서]
python manage.py runserver

[로컬서버 주소]
127.0.0.1:8000

# 에러메시지가 발생하면 로컬서버에서 친절히 알려주고,
# 터미널창 runserver도 같이 확인하는 것이 좋다.
```
<br>
<br>
> 경로설정 이해하기

```
[폴더 구조]
djangogirls-tutorial <-- 이 프로젝트 컨테이너 폴더(Root)
app <-- Django프로젝트 관련 코드 컨테이너 폴더
  blog
    migrations
  config
    settings.py
  templates
  .gitignore
  .git/
  .requirements.txt
```
<br>
Q. templates 폴더를 가리킬 수 있도록 설정
<br>
```

[루트폴더 설정]
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

BASE_DIR은 app(루트)폴더를 가리킨다

```
**영어 문법처럼 역순으로 해석한다.**
<br>
<br>
os.path.abspath(__file__)
app / config / `settings.py`를 가리킴
<br>
<br>
os.path.dirname
app / `config` / settings.py 파일을 가리킴
<br>
<br>
os.path.dirname
`app` / config / setting.py
<br>
<br>
```

[templates 경로 설정]
TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')

```
<br>
**역시 영어문법처럼 역순으로 접근해본다.**
(BASE_DIR, 'templates')
BASE_DIR기준으로 templates 로 내려간다.
<br>
<br>
os.path.join
config 와 templates는 동일한 등급이다. 그래서 join....이거 더 알아보자. 일단 가야할 길이 멀어서 skip
<br>
<br>
> views.py 파일과 settings.py 의 관계?

```python
from django.http import HttpResponse


html = render_to_string('blog/post_list.html')

return HttpResponse(html)
```
`render? HttpResponse?`
<br>
경로에 해당하는 HTML파일을 문자열로 로드해주고, 가져온 문자열을 돌려준다.
<br>
`내 맘대로 해석`
html 변수에 함수 render_to_string 함수를 이용하여 (templates/blog/post_list.html)에 렌더링을 해줘서 Http 방식으로 만들어주는 함수 HttpResponse를 사용했다.
<br>
<br>
<br>
아무튼 이렇게 적용한 함수들은 settings.py 에 적용시켜야 정상 작동을 한다.
<br>
<br>
그리고 views.py 파일 안에 (blog/post_list.html)은 templates라는 폴더를 사용자가 직접 만들었기 때문에 settings.py에 경로를 직접 등록해주어야 한다.
<br>
```
TEMPLATES = [
  {
      'DIRS':
        TEMPLATES_DIR, <--이거 등록
  }
]
```
> 여기까지 정리를 해보자면

저장할? 불러올? 폴더를 지정하고, post_list.html 파일을 local서버에서 불러올 수 있는 것까지 해보았다...
<br>
`commit message`
<br>
```
HTML 파일을 불러올 때 render_to_string을 사용

TEMPLATES_DIR을 지정 후, 해당 경로를 settings모듈의 TEMPLATES['DIR']에 추가
render_to_string에서 템플릿 파일을 찾기 위해 주어진 경로는 settings모듈의 TEMPLATES['DIRS']에 있는 폴더 목록을 기준으로 검색한다.
```
<br>
<br>
#### # 용어설명
<br>
HttpResponse
<br>
서버가 사용자의 요청에 대한 응답을 해주는 것
<br>
<br>
```
헤더   상태코드
      응답헤더
-------------
  공백부분
-------------
  본문

```
