---
layout: post
title: "[Design Pattern] 상태 패턴 (State)"
color: orange
author: janos
tags: [디자인패턴]
---

---

### 설명

위키백과에 나온 내용은 다음과 같다.

> 객체 지향 방식으로 state 기계를 구현하는 행위 소프트웨어 디자인 패턴이다. State Pattern을 이용하면 state pattern 인터페이스의 파생 클래스로서 각각의 state를 구현함으로써, 또 패턴의 슈퍼클래스에 의해 정의되는 메소드를 호출하여 상태 변화를 구현함으로써 상태 기계를 구현한다.
>
> 패턴의 인터페이스에 정의된 메소드들의 호출을 통해 현재의 전략을 전환할 수 있는 Strategy Pattern으로 해석할 수 있다. (`GoF 행위`에 포함)
>
> "위키백과 - [State](https://ko.wikipedia.org/wiki/%EC%83%81%ED%83%9C_%ED%8C%A8%ED%84%B4)"

- 어떤 행위를 수행할 때, state에 따라 수행되는 행위가 달라진다.
- Strategy Pattern과 비슷하다.
- State는 `다크 모드` / `라이트 모드` 같은 state에 따른 토글에 이용하면 좋을 것 같다고 생각이 들었다.

### 예시 코드

라이트, 다크 모드를 각가 클래스로 생성한 뒤에 모드를 관리하는 State 클래스를 사용하여 Window 클래스에서 현재 모드를 확인하고 변경하는 동작을 수행

```python
class State:
    def show_mode(self):
        print(f'현재 상태는 {self.mode} 모드 입니다.')

class LightState(State):
    def __init__(self, window):
        self.window = window
        self.mode = 'LIGHT'

    def toggle_DL(self):
        print('Swiching to Dark Mode')
        self.window.state = self.window.darkstate

class DarkState(State):
    def __init__(self, window):
        self.window = window
        self.mode = 'DARK'

    def toggle_DL(self):
        print('Swiching to Light Mode')
        self.window.state = self.window.lightstate

class Window:
    def __init__(self):
        self.lightstate = LightState(self)
        self.darkstate = DarkState(self)
        self.state = self.lightstate

    def toggle_DL(self):
        self.state.toggle_DL()

    def show_mode(self):
        self.state.show_mode()

if __name__ == '__main__':
    window = Window()
    actions = [window.show_mode] + [window.toggle_DL] + [window.show_mode]
    actions *= 2

    for action in actions:
        action()
        print('-'*20)
```

### 실행 결과

테스트는 아래의 절차를 2번 반복 진행하였다.

1. 현재 모드 확인
2. 상태 변환
3. 다시 현재 모드 확인

위의 절차를 진행했을 때, 모드의 상태이다.

1. 라이트 모드
2. 다크 모드
3. 다크 모드
4. 라이트 모드

![Result]({{ "/assets/img/post/2021-07-27-Designe-Pattern-State/상태-결과.png" | relative_url }})