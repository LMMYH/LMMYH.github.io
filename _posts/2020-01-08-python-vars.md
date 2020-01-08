---
title: "python - vars() 이용하여 클래스(메소드, 변수) 및 인스턴스(속성) 정보 dict로 반환"
date: 2020-01-08
tags:
  - Python
categories: "Python"
---

코드 살펴 보다가 간단히 정리해 두면 좋을 것 같아서 하는 python 첫 포스팅! 🤞<br>

우선 `vars()`에 대해 사용해본 건 변수를 문자열로 access할 때였다.<br>
`vars()['year'] = 2020` 이렇게 쓰는 건 `year = 2020` 랑 같게 된다.<br>

유용하게 쓸 법한 상황은 예를 들어,<br> 
for 문을 돌면서 변수를 다수 생성해야 하는데 변수명을 개별적으로 설정해줄 필요가 있을 때를 생각할 수 있겠다.<br>
다음처럼 1부터 연속하는 10개의 홀수를 x_1, x_2, ..., x_10에 쉽게 할당할 수 있다.<br>

```python
for i in range(10):
    vars()[f'x_{i+1}'] = 2*i + 1
```
<br>
⭐⭐⭐ 이번에 살펴볼 건, <br>
vars()를 통해 클래스와 인스턴스와 관련된 정보(변수, 속성 및 메소드)를 dict로 반환하는 방법이다.<br>
우선, class를 대충 하나 생성해 보겠다.<br>
<br>

```python
class Fruits:
    def __init__(self, name, brix, origin):
        self.name = name
        self.brix = brix
        self.origin = origin
    def get_grade(self):
        if self.brix >= 20:
            self.grade = 'A'
        else:
            self.grade = 'B'

jmt = Fruits('applemango', 21, 'Peru')
```
<br>
달달한 게 땡겨서 A급 페루산 애플망고를 데리고 왔다.<br>
`vars()` 괄호 안에 `jmt`를 넣으면 해당 인스턴스가 가진 속성들을 딕셔너리 형태로 반환해 준다.<br>
```python
vars(jmt)
```
<br>위의 코드를 수행하면, `{'name': 'applemango', 'brix': 21, 'origin': 'Peru'}` 다음과 같은 <br> 결과물을 얻을 수 있다. 이 때, 아직  `get_grade()` 메서드는 수행하지 않았기 때문에 grade에 대한 정보는 반영되지 않았다.<br>

```python
jmt.get_grade()
vars(jmt)
```
<br>`get_grade()` 메서드를 수행하고, 다시 정보를 조회해 보면 <br>다음과 같이 grade에 대한 정보도 포함된 딕셔너리를 얻을 수 있다.<br>
`{'name': 'applemango', 'brix': 21, 'origin': 'Peru', 'grade': 'A'}` 

<br> 이번엔, 클래스 자체를 조회해보면 어떻게 될까? <br>
```python
vars(Fruits)
```
<br>코드를 수행하게 되면
> `mappingproxy({'__module__': '__main__',
              '__init__': <function __main__.Fruits.__init__(self, name, brix, origin)>,
              'get_grade': <function __main__.Fruits.get_grade(self)>,
              '__dict__': <attribute '__dict__' of 'Fruits' objects>,
              '__weakref__': <attribute '__weakref__' of 'Fruits' objects>,
              '__doc__': None})`

위와 같은 아웃풋을 받아볼 수 있다. <br>
클래스에 대한 정보가 나와 있는데, 메서드에 대한 정보만 나와 있음을 알 수 있다. <br>
인스턴스 속성만 정의하고 클래스 변수는 따로 사용하지 않았기 때문이다.<br>

진부하지만 인스턴스를 만들 때마다 카운팅을 해주는 클래스 변수를 추가해 보고<br> 
다시 `vars(Fruits)`를 수행해 보겠다.<br>

```python
class Fruits:
    count = 0 # 추가
    def __init__(self, name, brix, origin):
        self.name = name
        self.brix = brix
        self.origin = origin 
        Fruits.count += 1 # 추가
    def get_grade(self):
        if self.brix >= 20:
            self.grade = 'A'
        else:
            self.grade = 'B'

vars(Fruits)
```
<br>코드를 수행하게 되면, 
> `mappingproxy({'__module__': '__main__',
              'count': 0,
              '__init__': <function __main__.Fruits.__init__(self, name, brix, origin)>,
              'get_grade': <function __main__.Fruits.get_grade(self)>,
              '__dict__': <attribute '__dict__' of 'Fruits' objects>,
              '__weakref__': <attribute '__weakref__' of 'Fruits' objects>,
              '__doc__': None})`

위와 같은 아웃풋이 나오는데, `'count': 0` 부분이 추가되었음을 알 수 있다. (아직, 인스턴스를 아무것도 생성하지 않았으므로 count는 0이 된다.)<br>


여러모로 쓰임이 있어서 기록해 두기 완료⭐! 




