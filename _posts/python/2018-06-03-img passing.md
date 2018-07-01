---
layout: post
title:  "BeautifulSoup 사용방법"
date:   2018-06-03
categories: python
comments: true
---
`웹사이트`  `이미지 크롤링 방법`  `BeautifulSoup`  `파싱하기`

**아래의 내용은 순수하게 개인의 학습을 목적으로 공부하는 자료입니다.**
<br>
**자료와 관련하여 저작권 침해 등의 문제 발생 시에는 바로 삭제를 하도록 하겠습니다.**
<br>
<br>
<br>
> 웹페이지 파싱하기

<br>
**단축키 알아가기**
<br>
`개발자 선택 도구`
<br>
```

Ctrl + Shift + C

```
<br>
`크롬에서 html소스 보기`
```

Ctrl + U

```
<br>
**필요한 모듈**
<br>
```
os모듈, requests모듈, lxml모듈, BeautifulSoup모듈
```
<br>
<br>
> 파싱이란
>> 웹에서 원하는 데이터만 추출하는 작업

<br>
**방법** 큰 범위(클래스 등)을 변수선언하고, 그 변수를 인덱싱으로 추출
```python
h2_title = soup.select_one('div.detail > h2')

title = h2_title.contents[0].strip()
author = h2_title.contents[1].get_text(strip=True)
```
**soup.select_one(selector)**
<br>
**selector[0].strip()**
<br>
**selector[1].get_text(strip=True)**
<br>
<br>
> 클래스가 없는 속성은?
>> for 문을 돌려서 추출

```
table = soup.select_one('table.viewList')
- 추출하고자 하는 속성의 가장 상위 클래스(viewList)확인
- 하위 속성들이 쭈~~~~욱 나온다.
- 추출하고자 하는 속성은 tr 이기 때문에 tr만 따로 추출해본다.

tr_list = table.select('tr')
- select 했기 때문에 viewList 안의 모든 tr 속성이 나온다.
- print 해보면 알겠지만 눈이 너무 아프다. enumerate로 tr을 기준으로 인덱싱해본다.

for index, tr in enumerate(tr_list):
  print('===={}===\n{}\n'.format(index, tr))
- 한결 보기 편해졌다.
```
<br>
table.viewList안에 'band banner'클래스가 있다. 이 클래스만 따로 빼고 싶다면?
```
for index, tr in enumerate(tr_list[1:]):
  cls = tr.get('class')
  print(cls)

(자주 출력해보는 것이 좋다. cls변수도 출력하기 위해서 생성한 변수이니까)
```
<br>
```
['band_banner', 'v2']
None
None
None
None
None
None
None
None
None
None
(실패했다...band_banner를 제외시키지 못했다.)
```
<br>
<br>
(그러다가 갑자기...뭔가...이해는 안되지만 band_banner class가 제외되서 출력됐다.)
```python
for index, tr in enumerate(tr_list[1:]):
  if tr.get('class'):
    continue
  print('========{} ====\n{}\n'.format(index, tr))
```
`continue`를 쓰는 이유는 `조건문`과 관련이 있다.
<br>
조건문에서 순회가 멈추는 조건은 `False`, `None`이다. class 만 추출하는데 현재 뽑아낸 html 소스에서 class는 존재하지 않는다.(즉, False로 선언)
<br>
만약 continue없이 실행시키면 첫번째 스텝에서 None값이 나와 조건문은 바로 종료가 될 것이다.
<br>
(html소스의 raw값을 구간별로 추출하기 위해!! 그리고 if 문을 끝까지 돌리기 위해서 continue를 사용)
<br>
continue를 없애고 출력해보니 정말로 첫번째 클래스만 추출되고 if문이 종료되었다.
<br>
<br>
> tr은 회차별로 추출되었으니 td값을 뽑아내면 된다.
>> 이제부터 시작이라는 의미이다.

<br>
<br>
html 에서 tr 속성이 뽑혔으니 td 속성만 추출하면 된다. 물론 말은 쉽다..
<br>
```
--------------------
tr1   td(썸네일)  td(회차)  td(별점)  td(등록일)
tr2   td(썸네일)  td(회차)  td(별점)  td(등록일)
--------------------
```
<br>
> 이제 섬네일을 뽑아보자.

tr > td > a href > img src 에 도달해야 썸네일을 뽑아낼 수 있다.
<br>
```python

tr.select_one('td:nth-of-type(1)_img')

```
<br>
1) `tr` 에서 _원하는 요소_ 만 뽑아내면 되기에 `select_one`을 사용
<br>
**tr.select_one**
<br>
<br>
2) 원하는 순번에 해당하는 태그만 뽑아내면 되기에 `nth-of-type`을 사용하고, get 메서드로 src 만 추출한다.
**td:nth-of-type(1) img').get('src')**
<br>
get('src')를 사용하는 이유는?
```
img src='url'주소 // title='내용' // alt='내용' width='71' // height='41'

왜 get('src')만 추출해야 하는지 자세한 내용은 생략하겠다.
```
<br>
<br>
`nth-of-type`: 해당하는 자식 태그 요소에서 순서를 찾음
`img`: image
`src`: source
<br>
<br>
