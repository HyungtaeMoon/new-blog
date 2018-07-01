---
layout: post
title:  "클래스(공격력, 방어력 증가)"
date:   2018-05-30
categories: python
comments: true
---
`코드 분석하기` `아이템을 사용한 강화`

```python
class User:
    def __init__(self, name, atk, defense):
        self.name = name
        self.atk = 0
        self.defense = 0
#         print(
#             f'({self.name})님이 생성되었습니다.\n'
#             f'현재 공격력은 {self.atk} 입니다.\n'
#             f'현재 방어력은 {self.defense}입니다.'
#         )

    def equip(self, item):
        item.use(self)
#         print(f'{self.name}님 공격력이 {self.atk} 올랐습니다.')
class sword:
    def use(self, user):
        user.atk += 10
        print(f'공격력이 {user.atk}가 증가하였습니다.')

class shield:
    def use(self, user):
        user.defense += 10
        print(f'방어력이 {user.defense}가 증가하였습니다.')
```

<br>

```
mht = User('문형태', 0, 0)

>>>(문형태)님이 생성되었습니다.
>>>현재 공격력은 0 입니다.
>>>현재 방어력은 0입니다.
```
<br>
<br>

```
클래스를 아래의 인스턴스에 할당

sword1 = sword()
defense2 = shield()

```

<br>
`첫번째 궁금증`
**__init__ 은 무엇이고, 메서드명을 다른 이름으로 지정하면  과연 돌아갈까?**
<br>
<br>
`기초답변` __init__ 의 뜻은 initialize로 '초기화 하다'의 뜻을 가지고 있다.
<br>
<br>
`내 멋대로 해석`외부에서 인스턴스.클래스 가 호출될 때 들어오는 값이 한 번 초기화를 시켜주어야 한다. 그래야 클래스 자체가 코딩한대로 돌아가기 때문이다. (동일한 클래스로 여러개의 인스턴스를 생성할 수 있다. 만약 인스턴스를 실행시킬 때마다 호출되면 인스턴스를 여러개 만들 수 없지 않은가.)
<br>
`실험1.` __hello__ 로 메서드명 생성
결과> 클래스 자체에는 오류가 발생하지 않는다.
<br>
그러나 인스턴스를 생성
<br>
(mht = User('문형태', 0, 0)하면 아래와 같은 오류가 발생한다.
<br>

```
TypeError                                 Traceback (most recent call last)
<ipython-input-19-67beda220de8> in <module>()
----> 1 mht = User('문형태', 0, 0)

TypeError: object() takes no parameters
```
(당장 내가 보고 해석할 짬은 안된다.)
<br>
<br>
`실험2` 인스턴스를 생성할 때 init 으로 초기화된다면, 인스턴스에 아무값이나 넣어도 될까?
test1) mht = User('문형태', 3, 5)
test2 mht = User('문형태', '', '')
<br>
`결과`코딩은 정상적으로 작동한다. 인스턴스를 생성할 때 1회 초기화가 되기 때문에 mht의 attribute 값은 class User 의 값에 대입이 된다.(**조금 더 생각해보기**)

```
def __init__(atk, defense):
self.atk = 0
self.defense = 0

그 어떤 숫자가 들어와도 모두 0으로 초기화한다.
```
<br>
<br>
<br>
**어떤 때는 () 로 호출하고, 어떤 때는 . 으로 호출하는데 그 차이점은? 그리고 메서드와 클래스, 클래스와 메서드는 어떻게 연결되는가?**

`생각1`
> sword1 = sword()
>> sword()의 메서드를 sword1 에 심어준다.
>
> mht.equip(sword1)
>> mht 인스턴스가 equip 메서드를 호출
>> equip 메서드는 sword1 인스턴스를 호출

<br>
<br>
```
mht.equip(sword1)

공격력이 30가 증가하였습니다. (equip 메서드)
문형태님 공격력이 30 올랐습니다. (sword 메서드)
```

역순으로 디버깅을 해본다.(완전 역순은 아니고)
<br>
1) mht.equip(sword1)를 호출
1-1) `mht`('문형태', 0, 0)
<br>
1-2) `equip` > class 접근 > 클래스 안의 equip 메서드에 접근
1-2-1)
<br>
1-3) `sword1` > 선언한 인스턴스 sword() 접근 > 클래스 sword 접근
<br>
