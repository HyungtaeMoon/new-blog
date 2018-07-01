---
layout: post
title:  "소지금 증가 클래스 만들기 "
date:   2018-05-27
categories: python
comments: true
---
`class`는 메서드(함수)의 묶음이다.
<br>
그렇다면 왜 클래스를 생성해야 하는가?
<br>
<br>
프로그램을 만들기 위해서는 역할별로 분류를 해놓고 코딩을 하는데,
메서드가 많으면 어디서부터 어디까지가 A의 역할을 하는지 확인하는데 시간이 많이 걸린다.
<br>
<br>
그래서 클래스를 생성해서 각 카테고리별로 구분을 해놓고 메서드를 정의해나간다.
<br>
<br>
# 클래스 분석하기(수업내용)
```python
class User:
    def __init__(self, name):
        self.name = name
        self.money = 0

    def add_money(self, amount):
        self.money += amount
        print(f'소지금이 {amount}만큼 증가하였다!')

    def sell_item(self):
        self.add_money(100)

    def kill_monster(self):
        self.add_money(50)
```
<br>
```
1) User라는 클래스를 선언하고 초기화 메서드(함수) init 선언

2) init 을 선언하는 이유는 속성(변수)값을 초기화시키고 그 위치에 아래쪽에 선언한 새로운 속성을 추가하기 위해서이다.

3) add_money 메서드의 역할은 money 값을 증가시키기 위한 최종 관문이다. (아래의 sell_item, kill_monster 모두 이 add)money를 통과해야만 한다.)

4) 메서드에 기본적인 공통점이 있다. 바로 (self) 자기 자신을 가리킨다는 것인데 '내 속성은 내가 수정한다'라고 생각해도 될 듯 하다.
```
어떻게 각각의 메서드를 연결시키지?
<br>

```
me = User('문형태')
#클래스형 인스턴스 생성
#초기화 메서드(__init__)이 실행되어 하나의 인자('문형태')를 전달
#self는 자동으로 채워지고, name에 '문형태'할당
```
<br>
```python
me.sell_item()
me.kill_monster()
me.money
#User 클래스가 심어져있는 me 에 sell_item(), 메서드 호출(부여)
```
<br>
<br>
>메서드끼리 상속하는 법은
>>1. 외부에서 인스턴스.메서드() 형식으로 호출하여 반영
>>2. 상위 인스턴스에서 설정한 인수를 하위 인스턴스에서 그대로 끌어다쓰기

<br>
---------------------------------------
<br>
`@property`
외부에서 사용할 수 없는 private 객체 속성을 파이썬에서는 property를 이용하여 상속
<br>
<br>
`super()`
부모 클래스 인스턴스의 내용을 자식 클래스에 상속하면서 속성을 추가로 생성
<br>
```python
#부모 클래스
@property
def info(self):
    return (
        f'상점정보 ({self.name})\n'
        f' 유형: {self._shop_type}\n'
        f' 주소: {self.address}'
    )
-----------------
#자식 클래스
@property
def info(self):
  abc = super().info
  return abc.replace('상점', '식당')
```
