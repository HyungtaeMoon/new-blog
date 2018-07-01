---
layout: post
title:  "Django에서 경로설정 확인하기"
date:   2018-06-06
categories: django
comments: true
---
`장고` `요청과 응답` `Get, Post 메서드`
<br>
<br>

#### # 웹서비스 개발에 주로 사용되는
#### # GET, POST 방식
<br>

>브라우저 주소창에 www.google.co.kr라는 url을 작성하고 엔터치면 구글 웹페이지가 나온다.


이 작업이 단순하게 **척하면 척하고** 이루어지는 것이 아니라 _많은 과정들을 거쳐서 우리에게 보여진다는 것_ 을 아주 조금 알게 되었다.
<br>
(정리한다고 했지만 현재는 정확하지도 않아 이 포스팅을 나중에 보면 전면 수정해야 할지도 모르겠다.)
<br>
<br>
### # get방식
로그인 후, 웹툰 페이지 등을 보면 url주소 뒤에 `?로 시작`하여 `딕셔너리` 값으로 주어지는 데이터들이 있다.
```

abcdeapple.com/gooood?titleId=345345&weekday=wed

```
<br>
이러한 방식이 `get 방식`이다. (응??? 끝???)
<br>
그러니까...`HTTP 패킷의 헤더에 포함`하여 서버에 요청을 한 후 페이지를 받아오는 방식이다.
<br>
<br>
즉, _get요청을 하면 서버는 자신이 받은 데이터를 분석_ 해서 DB에 쌓아놨다가 유저가 검색했을 때 `DB == 유저가 보낸 데이터가 일치`하면 DB를 보내주는 방식이다.
<br>
<br>
_처음에만 받은 DB를 저장하고 이후에는 저장한 데이터를 이용_ 하기 때문에 post방식보다는 속도가 빠르다.
<br>
<br>
<br>
#### 그래서 HTML 소스에서 어떻게 적용을 시키나?
<br>
> <form action={method="GET"}>

<br>
<br>
<br>
### # post방식
데이터 전송을 기반으로 한 메서드로 `html의 body에 데이터`를 넣어서 보내준다.
<br>
(이러한 이유로 보안성은 get보다 조금 더 낫다)
<br>
<br>
DB에 변화를 주는 동작은 post에 요청해야 한다.
<br>
<br>
post요청은 csrf token태그를 각 form안에 사용
<br>
<br>
### # post방식의 정의내리기

view function의 동작(#실습)
<br>
1. 오로지 `request.method가 'POST'일 때`만 동작<br>
  (request.method가 'GET'일 때는 아무 동작도 안 해도 됨)<br><br>
2. request.method가 'POST'일때의 동작<br>
   post_id에 해당하는 Post인스턴스에서<br>
   delete()를 호출해서 DB삭제<br>
   이후 post-list(url name)로 redirect
<br>
<br>

```python
if request.method == 'POST':
# request.method 가 POST 라면,

post = Post.objects.get(id=Post_id)
# Post_id에 해당하는 인스턴스를
# Post.objects.get으로 불러와
# post에 할당한다

post.delete()
# 할당한 후에는 삭제한다

return redirect('post-list')
# 위의 작업이 끝나면 리다이렉트 시킨다
```

<br>
#### 그래서 HTML 소스에서 어떻게 적용을 시키나?
<br>
> <form action={method="POST"}>

form을 정의할 때 method에 POST 라고 설정
<br>
<br>
### 한마디로
<br>
`get은 페이지를 보기만 할 때 사용하고,`
<br>
`post는 내용을 수정할 때 사용한다.`
<br>
<br>
### # 용어정리
#### 요청(request)
클라이언트가 서버에게 웹페이지를 보여달라고 말하는 것
<br>
<br>
#### 응답(response)
서버가 클라이언트에게 요청받은 것에 대한 대답으로 웹페이지 내용을 표현하기 이해 html문서로 주는 것
