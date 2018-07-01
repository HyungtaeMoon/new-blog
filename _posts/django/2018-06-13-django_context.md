---
layout: post
title:  "Django에서 context"
date:   2018-06-13
categories: django
comments: true
---
`적어두지 않으면 헷갈리는` `장고`
<br>
<br>
> 아직도 항상 멘붕인 장고 코딩 조각내서 분석하기

<br>
<br>
`blog/views.py`
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
    )
```
<br>
<br>
정확히 모르고 쓰는 함수 중에서 하나인 `context`는 settings.py파일에 `HttpRequest 객체를 가져와서` 딕셔너리 형식으로 반환하는 것 같다.(그렇다가 아니다 그런 것 같다이다.)
<br>
<br>
stackoverflow에서 찾아봤는데 분명 한국어로 번역해도 무슨 말인지 도통 모르겠다.
```
$ {myvar1 : 101, myvar2 : 102}

이 컨텍스트는 아래와 같이 전달이 된다

{{myvar1}} --> 101

{{myvar2}} --> 102
```
<br>
<br>
일단 모르는건 잠시 접어두고 공부를 하다가 에러메시지가 떠서 스크롤을 내려보니 각 요소별로 친절하게 직관적으로 확인을 할 수가 있었더라. `context`와 `posts`는 아래와 같이 확인되었다.
<br>
<br>
`context`
```
{'posts': <QuerySet [<Post: sample title second>, <Post: sample title>, <Post: [고든 정의 TECH+] 슈퍼컴퓨터 왕좌 되찾은 미국…앞으로의 과제는?>, <Post: [종합]이통3사, 지방선거·월드컵 대비 '이상무'>, <Post: "종이 증명 시대 끝…디지털트윈 실현될 것">, <Post: 삼성전자 '애플에 5800억원 배상' 美 평결에 재심 청구>, <Post: 무디스 "韓 통신비 인하 정책, 이통사 신용도 떨어진다">]>}
```
<br>
`posts`
```

<QuerySet [<Post: sample title second>, <Post: sample title>, <Post: [고든 정의 TECH+] 슈퍼컴퓨터 왕좌 되찾은 미국…앞으로의 과제는?>, <Post: [종합]이통3사, 지방선거·월드컵 대비 '이상무'>, <Post: "종이 증명 시대 끝…디지털트윈 실현될 것">, <Post: 삼성전자 '애플에 5800억원 배상' 美 평결에 재심 청구>, <Post: 무디스 "韓 통신비 인하 정책, 이통사 신용도 떨어진다">]><Quer
```
<br>
<br>
코드를 유심히 아니 대충만 봐도 눈치챘겠지만 posts 딕셔너리 형식을 변수 context에 담아서 return 시켰기 때문에 posts 와 context는 같은 값을 가지고 있다.
<br>
<br>
> 정말 마지막 결론

```
$ posts = Post.objects.order_by('-id')

내가 admin 페이지에서 적어두었던 포스팅 내용들을 호출해서 역순으로 정렬

$ context = {
  'posts': posts,
}
context 변수에 딕셔너리 형식으로 posts를 할당한다

return할 때는 context로 return한다

```
...........어렵다
