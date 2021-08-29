---
layout: post
title: "[Design Pattern] 어댑터 패턴 (Adapter)"
color: purple
author: janos
tags: [디자인패턴]
---

---

### 설명

항상 보던 위키백과를 보겠습니다.

> 클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스로 변환하는 패턴으로, 호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들이 함께 작동하도록 해준다. (`GoF 구조`에 포함)
> 
> "위키백과 - [Adapter](https://ko.wikipedia.org/wiki/%EC%96%B4%EB%8C%91%ED%84%B0_%ED%8C%A8%ED%84%B4)"

- 가장 설명이 간결한 디자인 패턴 같습니다.
- 기존 구조에 외부 기능을 접목시킬 때, 중간자 역할을 구성하는 방식입니다.

### 예시 코드

위키백과의 예제를 가져왔습니다.

```python
class Target(object):
    def specific_request(self):
        return 'Hello Adapter Pattern'

class Adapter(object):
    def __init__(self, adapter):
        self.adapter = adapter

    def request(self):
        return self.adapter.specific_request()

client = Adapter(Target())
print(client.request())
```

### 결과

Target 클래스의 specific_request() 함수를 바로 호출할 수 없어서 Adapter 클래스의 request() 함수를 통해 Target 클래스의 specific_request() 함수를 호출했습니다.

![Result]({{ "/assets/img/post/2021-08-05-Designe-Pattern-Adapter/어댑터-결과.png" | relative_url }})