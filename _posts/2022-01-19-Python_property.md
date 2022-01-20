---
title: "파이썬 @property"
excerpt:

categories:
  - Python
tags:
  - [Python, Effective Python, Theory]

toc: true
toc_sticky: true

date: 2022-01-19
last_modified_at: 2022-01-19
---

클래스에서 메서드를 통하여 속성의 값을 가져오거나 저장하는 경우가 있습니다. 이때 값을 가져오는 메서드를 getter, 값을 저장하는 메서드를 setter라고 부릅니다.

```python
class Person:
    def __init__(self):
        self.__age = 0

    def get_age(self):           # getter
        return self.__age

    def set_age(self, value):    # setter
        self.__age = value

james = Person()
james.set_age(20)
print(james.get_age())

>>>
20
```

파이썬에서는 @property를 사용하면 getter, setter를 간단하게 구현할 수 있습니다.

```python
class Person:
    def __init__(self):
        self.__age = 0

    @property
    def age(self):           # getter
        return self.__age

    @age.setter
    def age(self, value):    # setter
        self.__age = value

james = Person()
james.age = 20      # 인스턴스.속성 형식으로 접근하여 값 저장
print(james.age)    # 인스턴스.속성 형식으로 값을 가져옴

>>>
20
```

getter, setter 메서드의 이름을 잘 보면 둘다 age입니다. 그리고 getter에는 @property가 붙어있고, setter에는 @age.setter가 붙어있습니다. 즉, 값을 가져오는 메서드에는 @property 데코레이터를 붙이고, 값을 저장하는 메서드에는 @메서드이름.setter 데코레이터를 붙이는 방식입니다.

특히 @property와 @age.setter를 붙이면 james.age처럼 메서드를 속성처럼 사용할 수 있습니다. 값을 저장할 때는 james.age = 20처럼 메서드에 바로 값을 할당하면 되고, 값을 가져올 때도 james.age처럼 메서드에 바로 접근하면 됩니다.

```python
james.age = 20      # 인스턴스.속성 형식으로 접근하여 값 저장
print(james.age)    # 인스턴스.속성 형식으로 값을 가져옴
```

<br>

### @perperty를 데이터 검증에 활용

@property를 사용해서 age에 대한 검증을 수행할 수 있다.

```python
class Person:
    def __init__(self):
        self.__age = 0

    @property
    def age(self):           # getter
        return self.__age

    @age.setter
    def age(self, value):    # setter
        if age <= 0 :
            raise ValueError("Age는 0보다 큰 값이어야 합니다")
        self.__age = value

james = Person()
james.age = 20      # 인스턴스.속성 형식으로 접근하여 값 저장
print(james.age)    # 인스턴스.속성 형식으로 값을 가져옴

>>>
20
```



### 참조

- https://docs.python.org/3/library/functions.html#property
- https://www.nemonein.xyz/2021/03/4980/
- https://velog.io/@uoayop/3-3.-Propertygetter-setter
- https://velog.io/@kksh1205/python-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5%EA%B3%BC-property-%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0