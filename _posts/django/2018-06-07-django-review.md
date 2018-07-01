---
layout: post
title:  "쿼리 만들기(참고: 장고문서)"
date:   2018-06-07
categories: django
comments: true
---
`Django`
<br>
<br>
#### # 필요한 패키지 설치 내용

pip list를 쳐보면 현재 설치된 패키지의 버전이 나온다.
다른 개발자가 작성한 파일을 `동일한 환경에서 실행`시킬 수 있도록 `버전 정보`를 남겨놔야 한다
```
pip freeze > requirements.txt
```
`파일명은 requirements 라는 단어를 관례적으로 사용한다.`
<br>
<br>
```
python manage.py makemigrations
- 데이터 변경사항을 만들어준다.

python manage.py migrate
- 데이터 변경사항을 적용해준다.
```
<br>
<br>
#### # Django에서 처리 가능한 하나의 페이지를 만들 때 최소구현
<br>
`하나` url (특정 페이지의 url을 설정)
<br>
`둘` view (url로 들어온 request를 처리하고 response를 돌려줄 함수))
<br>
<br>
<br>
#### # 나는 하나의 파일만 `migration`했는데 많이 설치된다.?
<br>
_A. INSTALLED_APPS의 나머지 패키지들도 같이 설치가 되서 그렇다._
<br>
<br>
<br>
#### # 왜 많은 데이터베이스 중에 `SQLite`인가?
<br>
데이터 베이스가 파일 1개로 이루어져있는데, 이것이 다른 데이터베이스와 비교하면 성능이 부족하다.
<br>
<br>
그러나 내부데이터를 쌓는데 굳이 많은 기능은 필요가 없기 때문에 SQLite 데이터베이스를 쓴다.
<br>
<br>
settings.py 에 설치한 패키지(python manage.py startapp blog 등)과 같은 내용을 추가해야 한다.
<br>
```
python manage.py startapp [생성할 패키지명]
python manage.py startapp blog

그냥 장고 안에 패키지를 생성했다. 아직 Django와는 연동상태가 아니다.
```
`settings.py / INSTALLED_APPS에 추가`해야만 장고가 관리하도록 해준다.(필수라고 생각하자)
<br>
<br>
<br>
아래는 클래스 속성이다.
#### #app/blog/migrantions/models.py
```python
class Post(models.Model):
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )
    title = models.CharField(max_length=200)
    text = models.TextField(blank=True)
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

```
`class Post(models.Model)`
models안에 있는 Model을 참조한다.(상속)
<br>
`title = models.CharField(max_length=200)`
<br>
어떤 클래스를 호출했을 때 그 결과 인스턴스를 클래스 속성으로 넣었다. 이렇게 해놓은 이유는 데이터베이스와 매칭시키기 위해서 구성했다.(자세한건 복잡하니 pass)
<br>
```
author  title  text  create_date  published_date
작가     제목    내용     생성일         발행일
```
필드 5개가 있는 빈 시트를 구성하는게 위 클래스 모델이 하는 일이다.
<br>
<br>
#### #config/urls.py
**url resolver: url을 해석하는 부분**
```python
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'', include('blog.urls')),
]
```
`url(이 조건이 맞다면, 매칭되면 해당 사이트로 연결)`
url(r'^admin/', admin.site.urls')
<br>
<br>
`r''은 모든 매칭조건 OK, include(모듈명(또는 경로))`
`이 서버에 접속하는 유저를 blog/urls 경로로 안내한다`
`없다고 나오면 이 경로에 urls.py를 만들면 된다 ^^`
url(r'', include('blog.urls'))
<br>
```
http://localhost:8000
r'^admin/'
admin 페이지와 매칭되면 접속한다.
```
<br>
#### # blog/urls.py
```python
urlpatterns = [
    # url의 첫 번째 인자: 매치될 URL정규표현식
    # url의 두 번째 인자: view function
    #   view function
    #    -> request를 받아서 response를 돌려주는 함수

    # blog.views에 있는 post_list함수를
    # 아래 url함수의 두 번째 인자로 전달
    #  (함수호출 아님)
    url(r'^$', post_list, name='post-list'),
    # ex1) 3/
    # ex2) 235/
    # 정규표현식에 그룹을 지정해서 view function의
    #  인수로 전달한다
    url(r'^(\d+)/$', post_detail, name='post-detail'),
    url(r'^(\d+)/delete/$', post_delete, name='post-delete'),
    url(r'^(\d+)/edit/$', post_edit, name='post-edit'),
    url(r'^write/$', post_create, name='post-create'),
]
```
`url(매칭되는 정규표현식, blog/views.py, views.py의 매칭변수))`
url(정규표현식, view function, 매칭할 메서드명)
<br>
<br>
<br>

> url(정규표현식, view function, 매칭할 메서드명)

`view function`은 request를 받아서 response 해주는 함수
<br>
<br>
<br>
#### #blog/views.py
**그래서 인자에는 (request)를 넣어준다.**
```python
def post_list(request):
    posts = Post.objects.order_by('-id')
    context = {
        'posts': posts,
    }
  return render(
        request=request,
        template_name='blog/post_list.html',
        context=context,
```

<br>
<br>
<br>
#### # 전체 사이클 다시 되짚어보기
Browser -> request(localhost:8000) -> config.urls -> r'^$'에 매칭 -> def post_list -> return값을 다시 Browser
<br>
<br>
<br>
#### #파이썬 파일마다 from..import..??

```python
from django import models
from django.http import HttpResponse
from .models import Post
```
`from (가져올 경로) import 적용할 클래스, 메서드 등`
<br>
<br>
오늘은 이만 끄읏
