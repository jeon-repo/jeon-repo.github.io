---
layout: post
title: "[Design Pattern] 커맨드 패턴 (Command)"
color: blue
author: janos
tags: [디자인패턴]
---

---

### 설명

위키백과에 나온 내용은 다음과 같습니다.

> 요청을 객체의 형태로 캡슐화하여 사용자가 보낸 요청을 나중에 이용할 수 있도록 메서드 이름, 매개변수 등 요청에 필요한 정보를 저장 또는 로깅, 취소할 수 있게 하는 패턴이다. (`GoF 행위`에 포함)
> 
> "위키백과 - [Command](https://ko.wikipedia.org/wiki/커맨드_패턴)"

- Strategy Pattern과 비슷합니다.
- Strategy는 같은 일을 알고리즘이나 방식을 바꿔서 수행, Command는 하는 일 자체가 다릅니다.
- 각각 명령을 객체의 형태로 작성하는 느낌이라 명령의 조합이 필요한 `게임 캐릭터`의 컨트롤 같은 상황에서 사용하면 좋을 것 같다고 생각이 들었습니다.

### 예시 코드

janos라는 Warrior 클래스의 객체를 생성하고, 창과 해머 무기에 따라서 다른 공격 모션을 수행

```python
import abc

class Warrior:
    def __init__(self):
        self.swapCommand = []
        self.attackCommand = []

    def setCommand(self, swapCommand, attackCommand):
        self.swapCommand.append(swapCommand)
        self.attackCommand.append(attackCommand)

    def swapButton(self, slot):
        self.swapCommand[slot].execute()
    
    def attackButton(self, slot):
        self.attackCommand[slot].execute()

class Spear:
    def swap(self):
        print('해머에서 창으로 변경')

    def attack(self):
        print('찌르기 공격')

class Hammer:
    def swap(self):
        print('창에서 해머로 변경')

    def attack(self):
        print('휘두르기 공격')

class Command:
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def execute(self):
        pass

class SpearSwapCommand(Command):
    def __init__(self, spear_obj):
        self.spear = spear_obj

    def execute(self):
        self.spear.swap()

class SpearAttackCommand(Command):
    def __init__(self, spear_obj):
        self.spear = spear_obj

    def execute(self):
        self.spear.attack()

class HammerSwapCommand(Command):
    def __init__(self, hammer_obj):
        self.hammer = hammer_obj

    def execute(self):
        self.hammer.swap()

class HammerAttackCommand(Command):
    def __init__(self, hammer_obj):
        self.hammer = hammer_obj

    def execute(self):
        self.hammer.attack()

if __name__ == '__main__':
    janos = Warrior()

    spear_obj = Spear()
    hammer_obj = Hammer()

    spearSwapButton = SpearSwapCommand(spear_obj)
    spearAttackButton = SpearAttackCommand(spear_obj)
    hammerSwapButton = HammerSwapCommand(hammer_obj)
    hammerAttackButton = HammerAttackCommand(hammer_obj)

    janos.setCommand(spearSwapButton, spearAttackButton)
    janos.setCommand(hammerSwapButton, hammerAttackButton)

    janos.swapButton(0)
    janos.attackButton(0)
    janos.swapButton(1)
    janos.attackButton(1)
```

### 결과

착용한 무기가 창인 경우는 `찌르기`, 해머인 경우는 `휘두르기` 공격이 수행되고, 서로 다른 무기로 스왑이 진행됩니다.

![Result]({{ "/assets/img/post/2021-08-04-Designe-Pattern-Command/커맨드-결과.png" | relative_url }})