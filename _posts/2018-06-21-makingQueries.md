---
layout: post
title:  "쿼리 만들기(참고: 장고문서)"
date:   2018-06-22
categories: django
comments: true
---
`수업정리` `Query`
<br>
<br>
### # 개념설명
<br>
`QuerySet`은 실제로 데이터베이스에 도달하지 않고 구성, 필터링, 슬라이스 및 일반적으로 전달 될 수 있습니다. __쿼리셋을 평가할 때까지 실제로 데이터베이스 활동__ 이 발생하지 않습니다.
<br>
<br>
```
$ q1 = Pizza.objects.all()
데이터베이스를 호출하지 않고 쿼리셋만 만든 상태

$ q1
q1인스턴스를 호출한 바로 이 상태에서 비로소 데이터베이스에 다녀온다.
```
<br>
<br>
데이터 모델을 생성하면 Django는 자동으로 객체를 생성, 검색, 업데이트 및 삭제할 수 있는 데이터베이스 추상화 API를 제공
<br>
<br>
**용어설명**
```
API: Application Programming Interface

ORM: Object Relational Mapping
(Django ORM은 Class를 정의하고 레코드를 생성하는 작업을 의미)

QuerySet:
objects를 사용하여 다수의 데이터를 가져오는 함수를 사용할 때 반환되는 객체가 QuerySet
```

<br>
<br>
Django는 Python객체에서 데이터베이스 테이블 데이터를 나타내기 위해 직관적인 시스템을 사용
<br>
```python
Model Class는 데이터베이스 테이블

Class의 인스턴스는 데이터베이스 테이블의 특정 레코드


class Pizza(models.Model):
    name = models.CharField(max_length=128)
    toppings = models.ManyToManyField(
        Topping,
        related_name='pizzas',
    )

```
- `Pizza 클래스는 데이터베이스 테이블`
- `p1 = Pizza.objects.create(name='콤비네이션')`
- `p1이라는 데이터베이스 테이블의 특정 레코드 생성`
<br>
<br>
<br>
>**데이터베이스는 조금 수동적이다**

```python
$ combination
<Pizza: 하와이안>

$ Pizza.objects.all()
<QuerySet [<Pizza: 하와이안>, <Pizza: 콤비네이션>, <pizza: 페퍼로니>]>

$ combination.name = '콤비네이션1'

$ Pizza.objects.all()
<QuerySet [<Pizza: 하와이안>, <Pizza: 콤비네이션>, <pizza: 페퍼로니>]>

왜?? 아직 데이터베이스에 변경사항을 저장하지 않았기 때문이다.

$ combination.save()

$ Pizza.objects.all()

$ Pizza.objects.all()
<QuerySet [<Pizza: 콤비네이션1>, <Pizza: 콤비네이션>, <pizza: 페퍼로니>]>
```
**save()메서드를 사용하기 전까지는 데이터베이스 테이블에는 변화가 없다**
<br>
**즉, save()메서드가 데이터베이스에 변경사항을 적용한다고 선언**
<br>
<br>
<br>

> **"그러나 ManyToMany 필드에서 add()메서드만 사용하면 save()메서드까지 같이 사용한 것으로 인정한다"**
>> `add() = 레코드생성 + save()`

<br>
<br>
### # Many To Many 필드
<br>
**`pizza 테이블에서 topping 테이블에 호출`**
<br>
**`(중계테이블에 레코드 추가)`**
```python
$ combination.toppings.add(olive, onion)


$ combination.toppings.all()
<QuerySet[<Topping: 올리브><Topping: 양파>]>

```
<br>
**topping테이블아!!, 나 pizza테이블인데 중계테이블에 olive 좀 보내줄 수 있겠어?"**
<br>
<br>
라고 해서 생성된 테이블은 아래와 같다.
<br>
(표 수정할 것)
```
pizza		|	pizza_toppings  	|	topping
------------------------------------------------
콤비네이션 |	콤비네이션 | 올리브	   |	올리브
        |콤비네이션  | 양파		  |	  양파


$ combination.toppings.all()
중계테이블에서 콤비네이션피자에 있는 토핑들을 모두 알려줘
```


<br>
<br>
> **Retrieving objects(객체 검색하기)**

모델의 Manager를 사용하여 QuerySet을 얻는다. 각 모델에는 하나 이상의 Manager가 있으며 기본적으로 object라고 한다.
<br>
그래서 shell을 실행할 때도 ./manage.py shell 이고 ./manage.py startproject 라는 것들도 다 manage가 들어가는걸 보니 Manager가 중요하기는 한듯
<br>
<br>
> **Retrieving specific objects with filters(필터를 사용하여 특정 객체 가져오기)**

```python
filter
지정된 검색 매개 변수와 일치하는 객체가 포함 된 새 QuerySet을 반환

$ Entry.objects.filter(pub_date__year=2006)
Entry테이블에서 pub_date의 year이 2006년도인 레코드를 불러줘
언더바 2개는 '~의'로 해석


exclude
지정된 조회 매개 변수와 일치하지 않는 객체가 포함 된 새 QuerySet을 반환

$ Entry.objects.exclude(pub_date__year=2006)
Entry테이블에서 pub_date의 year이 2006년도를 제외한 레코드를 불러줘

```
<br>
<br>
> **Filtered QuerySets are unique(필터링 된 QuerySet은 고유합니다)**

<br>
```python
$ q1 = Entry.objects.filter(headline__startswith="What")
$ q2 = q1.exclude(pub_date__gte=datetime.date.today())
$ q3 = q1.filter(pub_date__gte=datetime.date.today())
```
**`q2에 q1의 QuerySet`을 넣었는데 이것은 복사가 아니라 새로운 레코드를 만든다.**
<br>
<br>
**즉, `q1을 다른 필터링을 걸어도 q2의 값은 변하지 않는다는 뜻`**
<br>
<br>
<br>
### # 한꺼번에 인스턴스를 생성하는 방법
```python
u1, u2, u3 = [BlogUser.objects.create(name=name) for name in ('김사람', '박인간', '박동물')]
```
<br>
**여러개의 레코드가 있을 경우에 뒤에서 3개만 선택**
```
u1, u2, u3, = [BlogUser.objects.order_by('-id')[:3][::-1]

u1 ~ [:3]까지는 데이터베이스에서 작업
[::-1]은 파이썬에서 작업
```
