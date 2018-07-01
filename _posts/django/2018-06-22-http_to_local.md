---
layout: post
title:  "Django Local에서 Http까지"
date:   2018-06-22
categories: django
comments: true
---
<br>
### # django에서 url연결하기
<br>
Django라는 프레임워크와 http를 연결하기 위해서 알아야할 선행 조건은 다음과 같다.
>`사용자 요청 > http(request) > django > 사용자가 페이지 확인`

<br>
#### # 단순하게 페이지뷰만 보여주기

###### (django와 http만 다뤘기 때문에 상세내용은 제외)

```
1. 사용자가 웹 페이지에서 주소 입력 후 엔터

2. http는 이 주소를 받고 django에 전달

2. urls.py는 받은 주소를 판단해서 해당 views.py로 전달

3. views.py는 생성한 메서드를 urls.py에 전달

4. urls.py는 받은 정보를 http에 전달

5. [http]는 [사용자]에게 전달받은 데이터를 보여줌

[전체 사이클]
- http(request) > url > views > url > http > 사용자
```
<br>
#### # urls.py 분석하기

urls.py는 **urlpatterns** 를 분석해서 `DJango`와 `http`를 연결해주는 중재자 역할을 한다.
<br>
<br>
`주의사항`
<br>
- path에서 url경로의 마지막에 '/'는 반드시 입력해줘야 한다.(빈 경로 제외) 그렇지 않다면 패턴을 인식하지 못해 `404`에러가 발생

<br>
총 3개의 패턴으로 이루어지는데, 하나씩 분석을 해보면 아래와 같다.
<br>
```python
urlpatterns = [
  path('admin/', admin.site.urls),
  path('posts/', include('posts.urls')),
  path('', views.index)
]
```
<br>
> #### 패턴.1

**admin 페이지에 접근하기(기본 생성)**

```python
urlpatterns = [
  path('admin/', admin.site.urls)
]
```
관리자 페이지인 admin 사이트에 연결하기 위한 경로이다. 들어가는 경로는 `localhost:8000/admin` 등의 방법으로 접근하여 데이터를 추가, 수정할 수 있다.
<br>
<br>
**관리자페이지 계정 생성방법**
```python
./manage.py createsuperuser
```
<br>
<br>
> #### 패턴.2

**다른 urls.py로 연결**
<br>
<br>
페이지가 많아질수록 **한꺼번에 관리하기가 쉽지 않다**. 그래서 각 카테고리별로 폴더를 나눠놓고 `urlpatterns`에서 `include`를 사용했다.
<br>
- config/urls.py(http와 연결되는 파일)
- posts/urls.py(위 파일의 작업을 분산받음)

```python
[config/urls.py]

urlpatterns = [
  path('posts/', incldue('posts.urls'))
]

[posts/urls.py]
urlpatterns = [
  path('login/', views.login_view, name='login')
]

```
**패턴분석**
<br>
```
[config/urls 패턴 분석]

path('http 접속경로/', incldue('디렉토리명.urls'))
* 해당 접속경로는 include 된 경로를 이용하세요
```

##### `웹주소 **localhost:8000/posts/**` 와 `posts디렉토리`의 url를 연결해주는 역할을 한다.

##### **posts.urls.py** 라고 입력하면 모듈로 인식하지 못해서 에러가 발생하니 주의하자.

<br>
<br>
```
[posts/urls 패턴 분석]

path('접속되는 주소', views.메서드명, name=메서드명)
```
