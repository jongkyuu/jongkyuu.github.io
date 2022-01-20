---
title: "파이썬 약한참조"
excerpt:

categories:
  - Python
tags:
  - [Python, Effective Python, Theory]

toc: true
toc_sticky: true

date: 2022-01-20
last_modified_at: 2022-01-20
---

파이썬에서 실행시간에 생성된 객체는 자동으로 관리되어 “더 이상 쓸모가 없어지면” 자동으로 파괴된다. 따라서 파이썬에서는 명시적으로 객체를 파괴하는 코드를 작성하지 않는 것이 보통이다.   
파이썬은 Objective-C와 비슷하게 참조수(Reference Count)기반의 자동 메모리 관리 모델을 따르고 있다. 파이썬의 모든 변수는 값을 담는 영역이 아니라 객체에 바인딩 되는 이름이다. 객체와 이름이 바인딩되면, 해당 객체는 그 이름에 의해 참조된다. 모든 객체는 참조 당할 때 레퍼런스 카운터를 증가시키고 참조가 없어질 때 카운터를 감소시킨다. 이 카운터가 0이 되면 객체가 메모리에서 해제한다(가비지 콜렉터의 도움 없이). 어떤 객체의 레퍼런스 카운트를 보고 싶다면 sys.getrefcount()로 확인할 수 있다.


### 객체가 제거될때 메시지 남기기

객체가 참조수 0이 되거나 다른 이유로 메모리에서 해제될 때에는 해당 객체의 __del__() 메소드가 호출된다. 쉘에서 del foo 라고 했을 때에도 foo.__del__()이 호출된다. 우리는 특정 클래스에서 이 메소드를 변경해서 객체가 제거되는 시점에 필요한 리소스 정리를 할 수 있다.

```python
class Foo:
  def __init__(self):
    self.value = 1
  def __del__(self):
    print(f'Object({id(self)}:{self.__class__}) is being destroyed.')
    
## 쉘에서 테스트
>> a = Foo()
>> a = 1  ## 1)
Object(1087b819c18:<class '__main__.Foo'>) is being destroyed.
```

위 코드를 보면 __del__() 메소드를 정의하여 객체가 삭제될 때 자신이 파괴된다는 메세지를 출력하도록 했다. 그리고 실제로 인스턴스를 생성한 다음, a라는 이름에 바인딩했다. 곧이어 a라는 이름을 다른 객체, int 타입 1이라는 객체에 바인딩하게 되면 처음에 Foo()로 생성된 객체는 자신을 참조하는 이름이 0개가 되고 곧 GC에 의해서 제거된다. 


```python
>> a = Foo()
>> b = Foo()
>> a = b  ## 1)
Object(1087b8192b0:<class '__main__.Foo'>) is being destroyed.
>> b = 1  ## 2)
>> a = 2  ## 3)
Object(1087b827ba8:<class '__main__.Foo'>) is being destroyed.
```

두 개의 객체 인스턴스를 생성하고 이름 a를 이름 b가 가리키고 있는 객체로 바인딩한다. 그러면 처음에 a 이름으로 생성된 객체는 이 시점에 파괴되는 것을 확인할 수 있다. (1)) 다음으로 이름 b를 다른 객체로 바인딩한다. b 라는 이름으로 생성된 객체는 이름 a에 의해서 여전히 참조되고 있으므로 아직 파괴되지 않는다. (2)) 다시 남은 이름 a가 다른 객체를 가리키게 되는 시점에 두 번째로 생성한 객체가 파괴되었다. (3))


### 메모리가 누수되는 Case

여기까지 보면 이 모델에서 큰 문제점을 느끼지 못할 수도 있다. 하지만 다음 예제를 살펴보자.

```python
>> a, b = Foo(), Foo()
>> a
<__main__.Foo object at 0x000001D953A38BE0>
>> b
<__main__.Foo object at 0x000001D953A38BA8>
>> a.friend = b  ## 1
>> b = None      ## 2
>> a = None      ## 3
Object(at 1d953a38be0, <class '__main__.Foo'>) is being destroyed. 
```

두 개의 Foo 타입 객체 a, b를 만들고 b를 a의 속성으로 만들었다(1).이후에 b의 바인딩을 바꾸어도 두 번째 객체는 제거되지 않는다(2). (왜냐면 a.friend라는 속성이 b 이름으로 생성된 객체를 참조하고 있다. 그 상태에서 a를 제거하면 원래 이름 a가 바인딩하던 첫번째 객체만 제거되었고, 원래 b였던 두 번째 객체는 제거되지 않았다. 왜냐하면 객체 a가 제거되는 과정에서 a.friend 에 대한 참조를 제거하는 작업이 전혀 진행되지 않았기 때문이다.

그렇다면 “죽을 때 속성을 정리하기”만 하면 괜찮을까? Foo 클래스의 __del__() 메소드를 약간 수정해서 다음과 같이 바꾸어서 테스트해본자.

```python
class Foo:
  def __init__(self):
    self.v = 1
    self.friend = None
  def __del__(self):
    self.friend = None
    print(f'{id(self):x} is destroyed.')
## 테스트
>> a, b = Foo(), Foo()
>> a.friend = b
>> b = None  ## 참조가 살아있기 때문에 파괴되지 않음
>> a = None
2520efc8b70 is destroyed.  
2520ef38630 is destroyed.
```

a에 대한 참조가 없어지면서 a.__del__()이 호출되고, 이 시점에 최초 b 였던 두번째 객체에 대한 참조가 모두 제거되면서 b 객체가 제거된다. 그리고 곧이어 a 객체도 제거되었다.

문제가 해결된것 같지만, 다음을 살펴보자.

```python
>> a = Foo()  # 0x60
>> b = Foo()  # 0xa8
>> a.friend = b  # 0x60의 x는 0xa8를 가리킨다.
>> b.friend = a  # 0xa8의 x는 0x60를 가리킨다.
# 이 시점에서 레퍼런스 카운터를 살펴보면
# 0x60의 레퍼런스 카운터는 a와 b.x로 2
# 0xa8의 레퍼런스 카운터는 b와 a.x로 2
>> b = None
>> a = None  ## 1)
```

두 객체가 각각 자신의 속성으로 서로를 참조하고 있다. 따라서 a = None이 호출되는 시점에 원래 a 였던 객체(0x60)의 참조수는 2에서 1로 줄어든다. 하지만 남은 참조를 가지고 있던 이름 b는 None을 가리키고 있고, b.friend 라는 속성 이름 자체에 대한 접근이 막혔다. 따라서 a.__del__() 이 호출될 수 없기 때문에 두 객체는 메모리에 계속 남아 메모리 누수가 발생하게 된다.

```python
>> c = Foo()
>> c.friend = c
>> c = None
```

c는 자기 자신의 속성 이름에 의해서 스스로 순환참조를 만들어버렸다. 따라서 c 역시 수동으로 c.friend=None 과 같은 식으로 먼저 속성에 의한 참조를 해제하지 않는 이상 이대로 메모리 누수를 일으키는 고립된 객체가 되고 만다.

<br>

### 약한 참조

약한 참조는 말 그대로 대상 객체를 참조는 하지만, 대상 객체에 대한 소유권을 주장하지 않는, 즉 reference count를 올리지 않는 참조를 말한다. 약한참조는 weakref 모듈의 ref 라는 클래스를 통해서 생성할 수 있다. 

- 특정 대상에 대해 약한 참조를 만들 때는 weakref.ref(target) 으로 ref()에 인자로 넘겨 약한참조 객체를 생성한다.
- 생성된 약한참조로부터 참조대상을 얻으려 할 때는 약한참조 객체를 호출한다.   


약한 참조는 대상에 대한 강한 참조를 유지하지 않기 때문에 위에서 언급한 문제에 대해서 구애받지 않는다. 참조하려는 대상이 파괴되었다면 약한참조는 참조 대상에 대한 액세스를 요청받을 때 None 을 리턴한다.

```python
class Foo:
  def __init__(self):
    self.value = 1
    self.friend = None
  def __del__(self):
    ## 여기서 딱히 friend 속성을 정리하지 않는다.
    print(f'Object({id(self):x}) is being destroyed.')

## 테스트
>> import weakref
>> a, b = Foo(), Foo()
## 각각의 객체를 약한 참조를 이용해서 할당한다.
>> a.friend = weakref.ref(b)
>> b.friend = weakref.ref(a)

## 객체를 지워본다.
>> b = None
Object(2520efedc18) is being destroyed.
>> a.friend()    ## 제거된 대상을 액세스하려하면 None이 리턴된다.
>> a = None
Object(2520efc8ba8) is being destroyed.

## 순환 참조에 대해서도 테스트
>> c = Foo()
>> c.friend = weakref.ref(c)
>> c = None
Object(2520efed518) is being destroyed.
```

약한 참조는 위와 같이 한 개 이상의 객체가 참조 순환 고리를 만드는 경우에 메모리 누수를 방지하기 위해서 주로 사용한다.    
클래스 인스턴스의 속성이 만약 다른 클래스의 인스턴스를 참조하거나, 동일 클래스의 다른 인스턴스를 참조할 가능성이 높다면 메모리 누수가 발생할 가능성이 매우 높은 지점이 된다. 대부분의 파이썬 프로그램의 생애주기는 짧기 때문에 문제가 되지 않을 수 있다. 하지만 서버와 같이 생애 주기가 길거나, 매우 많은 인스턴스를 생성하고 연결하는 동작을 하는 경우에 메모리 누수가 큰 문제가 될 수 있다.

<br>

### @property를 사용한 코드 개선

Foo 의 경우와 같이 커스텀 클래스 인스턴스를 속성을 사용하는 부분이 많다면 매번 weakref.ref(x) 를 쓰거나, obj.attr() 과 같이 호출하는 식의 코드를 작성하는 것은 피곤한 일이다. 객체 프로퍼티를 사용해서 접근자를 호출하는 식으로 우회하여 이 문제를 개선할 수 있다.

```python
import weakref
class ConvenientFoo:
  def __init__(self, value):
    self.value = value
    self._friend = None
 
  @property
  def friend(self):
    if self._friend is None:
      return None
    return self._friend()
  @friend.setter
  def friend(self, target):
    self._friend = weakref.ref(target)
  def __del__(self):
    print(f'{id(self):x} is being destroyed.') 
```