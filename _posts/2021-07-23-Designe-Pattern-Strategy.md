---
layout: post
title: "[Design Pattern] 전략 패턴 (Strategy)"
color: gray
author: janos
tags: [디자인패턴]
---

---

### 설명

위키백과에 나온 내용은 다음과 같습니다.

> 실행 중에 알고리즘을 선택할 수 있게 하는 행위 소프트웨어 디자인 패턴이다.
> 
> - 특정한 계열의 알고리즘들을 정의
> - 각 알고리즘을 캡슐화
> - 알고리즘들을 해당 계열 안에서 상호 교체가 가능하게 만듬
> 
> Strategy는 알고리즘을 사용하는 클라이언트와는 독립적으로 다양하게 만든다. Strategy는 유연하고 재사용 가능한 객체지향 소프트웨어를 어떻게 설계하는지 기술하기 위해 디자인 패턴의 개념을 보급시킨 디자인 패턴이라는 영향력 있는 책에 포함된 패턴들 가운데 하나이다. (`GoF 행위`에 포함)
> 
> "위키백과 - [Strategy Pattern](https://ko.wikipedia.org/wiki/전략_패턴)"

- 객체지향 프로그래밍 설계를 할 때 자주 발생하는 문제들을 피하기 위해 사용되는 패턴이라고 합니다.
- 위키백과의 설명을 보면 알고리즘의 캡슐화를 통해 실행중에 알고리즘을 바꿔가며 사용하는 방식입니다.
- Strategy는 `하나의 시스템`에서 `설정이 각각 다른 여러 모듈`을 사용할 때 설정의 `공통 부분`에 이용하면 좋을 것 같다고 생각이 들었습니다.

### 예시 코드

클래스 선언시에 parameter가 없으면 클래스 내부 함수 호출, parameter가 있으면 전달받은 함수 호출

```python
class Strategy():
    # Python은 함수도 parameter로 전달하고, 전달받을 수 있다.
    def __init__(self, obj=None):
        # obj가 있으면 self.getFunction()에 할당
        if obj:
            self.getFunction = obj

    def getFunction(self):
        return 'Internal Function'

def outFunction1():
    return 'External Function 1'

def outFunction2():
    return 'External Function 2'

if __name__ == '__main__':
    strategy0 = Strategy()
    strategy1 = Strategy(outFunction1)
    strategy2 = Strategy(outFunction2)

    print(f'strategy0: {strategy0.getFunction()}')
    print(f'strategy1: {strategy1.getFunction()}')
    print(f'strategy2: {strategy2.getFunction()}')
```

### 실행 결과

strategy0, 1, 2의 결과가 각각 다른 것을 확인할 수 있습니다.

![Result]({{ "/assets/img/post/2021-07-23-Designe-Pattern-Strategy/전략-결과.png" | relative_url }})