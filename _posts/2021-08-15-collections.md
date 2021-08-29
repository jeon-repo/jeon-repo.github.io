---
layout: post
title: "collections 라이브러리"
feature-img: "assets/img/feature-img/beach001.jpeg"
author: janos
tags: [Python]
---

---

`collections` 는 Python의 내장 라이브러리로 `컨테이너 데이터형` 의 구현 및 활용을 도와주는 라이브러리입니다.

`collections` 의 주요 객체를 살펴보면 아래와 같습니다.

| 객체        | 설명                                                                  |
|-------------|-----------------------------------------------------------------------|
| namedtuple  | 이름 붙은 필드를 갖는 튜플 서브 클래스를 만들기 위한 팩토리 함수      |
| deque       | 양쪽 끝에서 빠르게 추가와 삭제를 할 수 있는 리스트류 컨테이너         |
| ChainMap    | 여러 매핑의 단일 뷰를 만드는 딕셔너리류 클래스                        |
| Counter     | 해시 가능한 객체를 세는 데 사용하는 딕셔너리 서브 클래스              |
| OrderedDict | 항목이 추가된 순서를 기억하는 딕셔너리 서브 클래스                    |
| defaultdict | 누락된 값을 제공하기 위해 팩토리 함수를 호출하는 딕셔너리 서브 클래스 |

* TOC
{:toc}

# namedtuple

이름 그대로 튜플(값)에 이름(키)을 붙이는 데이터 구조입니다.

먼저 간단하게 사용 방법을 보겠습니다.

```python
from collections import namedtuple

# namedtuple 선언부분, []안에 이름(키)을 선언
Computer = namedtuple('Computer', ['cpu', 'ram', 'ssd'])
# 아래의 방식으로 이름(키) 선언 가능
Computer = namedtuple('Computer', 'cpu ram ssd')

# Computer라는 namedtuple 객체를 통해 데이터를 할당하여, com_100 인스턴스 생성
com_100 = Computer(cpu='i5', ram=8, ssd=1024)

# com_200 생성
com_200 = Computer(cpu='i7', ram=16, ssd=2048)

print(f'com_100 [type: {type(com_100)} / data: {com_100}]')
print(f'com_200 [type: {type(com_200)} / data: {com_200}]')
---------------------------------------------------
"com_100 [type: <class '__main__.Computer'> / data: Computer(cpu='i5', ram=8, ssd=1024)]"
"com_200 [type: <class '__main__.Computer'> / data: Computer(cpu='i7', ram=16, ssd=2048)]"
```

출력된 결과를 보면 타입이 **namedtuple로 선언된 객체**인 `Computer` 로 나오는 것을 볼 수 있습니다.

아래는 `namedtuple` 와 관련된 추가적인 내용입니다.

```python
# 출력부만 변경
# 이름(키)을 통해서 해당되는 데이터를 호출할 수 있다.
print(f'com_100 [ssd: {com_100.ssd}]')
print(f'com_200 [cpu: {com_100.cpu}]')
---------------------------------------------------
"com_100 [ssd: 1024]"
"com_200 [cpu: i5]"
```
```python
# _make(iterable)
amd = ['amd 5600x', 16, 2048]
com_amd_make = Computer._make(amd)
print(com_amd_make)
---------------------------------------------------
"Computer(cpu='amd 5600x', ram=16, ssd=2048)"
```
```python
# _fields
print(com_100._fields)
---------------------------------------------------
"('cpu', 'ram', 'ssd')"
```
```python
# _field_defaults
# -> default값은 뒷쪽부터 채워짐
# -> 인스턴스 생성시 입력값은 앞쪽부터 채워짐
Default_Computer = namedtuple('Default_Computer', ['cpu', 'ram', 'ssd'], defaults=[4, 1024])
com_100 = Default_Computer('i3')
com_200 = Default_Computer('i5')

# default로 설정된 값 확인
print(Default_Computer._field_defaults)
print(f'com_100: {com_100}')
print(f'com_200: {com_200}')
---------------------------------------------------
"{'ram': 4, 'ssd': 1024}"
"com_100: Default_Computer(cpu='i3', ram=4, ssd=1024)"
"com_200: Default_Computer(cpu='i5', ram=4, ssd=1024)"
```
```python
# namedtuple은 class로 취급되기 때문에,
# 서브 클래스를 사용하여 기능을 추가, 변경할 수 있다.
class Computer(namedtuple('Computer', ['cpu', 'ram', 'ssd'])):
    __slots__ = ()
    def __str__(self):
        # 출력 형태를 재선언하여 변경
        return f'Computer[cpu: {self.cpu}|ram: {self.ram}|ssd: {self.ssd}]'

com_test1 = Computer('i3', 4, 1024)
com_test2 = Computer('i5', 8, 2048)
print(com_test1)
print(com_test2)
---------------------------------------------------
"Computer[cpu: i3|ram: 4|ssd: 1024]"
"Computer[cpu: i5|ram: 8|ssd: 2048]"
```

# deque

deque는 스택과 큐를 일반화 한 것입니다.

`Double Ended Queue`의 약자입니다.

데이터의 삽입과 삭제가 양쪽에서 모두 가능한 자료구조입니다.

아래의 함수를 통해 활용이 가능합니다.

| 함수                  | 설명                                                                                                                                             |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| append(x)             | deque 오른쪽에 x를 추가                                                                                                                          |
| appendleft(x)         | deque 왼쪽에 x를 추가                                                                                                                            |
| clear()               | deque에서 모든 요소를 제거하고 길이가 0인 상태로 만듬                                                                                            |
| copy()                | deque의 얕은 복사본을 만듬                                                                                                                       |
| count(x)              | x와 같은 요소의 갯수를 반환                                                                                                                      |
| extend(iterable)      | iterable 인자의 요소를 추가하여 deque의 오른쪽을 확장                                                                                            |
| index(x, start, stop) | deque에 있는 x의 위치를 반환 (start 이후, stop 이전 - 필수 아님), 반환할 수 없는 경우 ValueError 발생                                            |
| pop()                 | deque의 오른쪽에서 요소를 제거하고 반환, 요소가 없으면 IndexError 발생                                                                           |
| popleft()             | deque의 왼쪽에서 요소를 제거하고 반환, 요소가 없으면 IndexError 발생                                                                             |
| remove(value)         | value의 첫 번째 항목을 제거, 찾을 수 없으면 ValueError 발생                                                                                      |
| reverse()             | deque 요소들의 순서를 뒤집기, 원본 데이터가 변형                                                                                                 |
| rotate(n=1)           | deque를 n단계 오른쪽으로 회전, n이 음수면 왼쪽으로 회전 → dq.rotate(n=1) == dq.appendleft(dq.pop()) → dq.rotate(n=-1) == dq.append(dq.popleft()) |
| maxlen                | deque의 최대 크기 or None 반환                                                                                                                   |

# ChainMap

맵 여러 개를 합쳐서 중복을 제외하고 하나의 dictionary로 만들어주는 자료구조입니다.

중복된 데이터는 첫 번째 인자로 입력된 맵의 데이터를 남겨두고 나머지는 제외합니다.

Map 형태의 데이터를 인자로 받아서 선언합니다.

```python
from collections import ChainMap

a = {1: 'a', 3: 'c1'}
b = {2: 'b', 3: 'c2'}

# 선언
cm = ChainMap(a, b)
print(cm)
print(list(cm.keys()))
print(list(cm.values()))
---------------------------------------------------
"ChainMap({1: 'a', 3: 'c1'}, {2: 'b', 3: 'c2'})"
"[2, 3, 1]"
"['b', 'c1', 'a']"
```

# Counter

요소와 요소의 갯수로 "**키-값**"의 형태를 같는 해시 가능한 dictionary 서브 클래스입니다.

기본적으로 특정 요소의 갯수를 셀 때 사용합니다.

아래의 코드를 보면 4가지 형태의 선언 방식을 사용할 수 있습니다.

```python
from collections import Counter

# Counter 객체의 인스턴스 생성
c = Counter()
c = Counter('word test')
c = Counter({'red': 4, 'blue': 2})
c = Counter(red=4, blue=2)
```

key 값에 갯수 확인을 원하는 요소를 넣으면 갯수를 확인할 수 있다.

```python
from collections import Counter

c = Counter(['red', 'blue'])
print(f"red: {c['red']}")
print(f"green: {c['green']}")
---------------------------------------------------
"red: 1"
"green: 0"
```

아래는 함수를 통해 사용할 수 있는 기능입니다.

```python
from collections import Counter

# elements() -> iter의 형태로 요소들을 반환
c = Counter(a=2, b=3, c=0, d=1)
print(c.elements())
print(sorted(c.elements()))
---------------------------------------------------
"<itertools.chain object at 0x7ffe51242040>"
"['a', 'a', 'b', 'b', 'b', 'd']"
```
```python
# most_common([n]) -> 갯수가 가장 많은 요소부터 n개 출력
c = Counter('abracadabra').most_common(3)
print(c)
---------------------------------------------------
"[('a', 5), ('b', 2), ('r', 2)]"
```
```python
# update([iterable-or-mapping]) -> 요소들의 합을 연산 (원본 데이터 변경)
c3 = Counter(a=3, b=2, c=1)
c4 = Counter(a=1, b=2, c=3)
c3.update(c4)
print(c3)
---------------------------------------------------
"Counter({'a': 4, 'b': 4, 'c': 4})"
```
```python
# subtract([iterable-or-mapping]) -> 요소들의 차를 연산 (원본 데이터 변경)
c1 = Counter(a=3, b=2, c=1)
c2 = Counter(a=1, b=2, c=3)
c1.subtract(c2)
print(c1)
---------------------------------------------------
"Counter({'a': 2, 'b': 0, 'c': -2})"
```

# OrderedDict

> Python 3.7 이후부터 기본 dictionary에서도 데이터 **입력 순서가 보존**되기 때문에 활용성이 약간 감소되었습니다.
> 
> "Python - [Dictionary]([https://docs.python.org/3.7/tutorial/datastructures.html#dictionaries](https://docs.python.org/3.7/tutorial/datastructures.html#dictionaries))"

현재 일반 dictionary 객체와의 차이는 데이터의 순서를 바꾸는 연산에 적합하다는 차이만 가지고 있습니다.

아래는 순서를 변경할 때 사용하는 함수입니다.

```python
from collections import OrderedDict

# move_to_end(key, last=True) -> last가 False이면 왼쪽 끝으로 이동
d = OrderedDict.fromkeys('abcde')
d.move_to_end('c')
print(f'True: {d}')
---------------------------------------------------
"True: OrderedDict([('a', None), ('b', None), ('d', None), ('e', None), ('c', None)])"
```
```python
d.move_to_end('c', False)
print(f'False: {d}')
---------------------------------------------------
"False: OrderedDict([('c', None), ('a', None), ('b', None), ('d', None), ('e', None)])"
```
```python
# popitem(last=True) -> last가 True면 LIFO, False이면 FIFO 순서로 반환
d = OrderedDict.fromkeys('abcde')
k, v = d.popitem()
print(f'True: {k, v}')
print(d)
---------------------------------------------------
"True: ('e', None)"
"OrderedDict([('a', None), ('b', None), ('c', None), ('d', None)])"
```
```python
k, v = d.popitem(False)
print(f'False: {k, v}')
print(d)
---------------------------------------------------
"False: ('a', None)"
"OrderedDict([('b', None), ('c', None), ('d', None)])"
```

# defaultdict

dictionary value의 초깃값을 설정합니다.

```python
from collections import defaultdict

default_list = defaultdict(list)
default_list[1]
default_list[2]
print(f'default_list: {default_list}')
---------------------------------------------------
"default_list: defaultdict(<class 'list'>, {1: [], 2: []})"
```
```python
default_dict = defaultdict(dict)
default_dict[1]
default_dict[2]
print(f'default_dict: {default_dict}')
---------------------------------------------------
"default_dict: defaultdict(<class 'dict'>, {1: {}, 2: {}})"
```

> "Python 3.9.6 Documentation - [collections](https://docs.python.org/ko/3/library/collections.html#collections.namedtuple)