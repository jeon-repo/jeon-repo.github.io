---
layout: post
title: "[Design Pattern] 싱글턴 (Singleton)"
feature-img: "assets/img/feature-img/pink-cloud-moon.jpg"
author: janos
tags: [디자인패턴, Algorithm]
---

---

### [설명]

먼저 위키백과의 설명을 한 번 보겠다.

> 클래스의 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고, 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴하는 방식의 디자인 유형을 Singleton 패턴이라고 한다.
> 
> "위키백과 - [Singleton Pattern](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80%ED%84%B4_%ED%8C%A8%ED%84%B4)"

- 위키백과의 설명처럼 특정 객체를 여러곳에서 공유하기 위해서 사용하는 방식같다.
- Python은 모듈 자체가 Singleton으로 구성되어있다.  
→ 클래스 변수를 생성하면 해당 클래스의 객체가 생성된 어느 곳이던 같은 변수값이 반환된다.
- Sigleton은 `다크 모드` 같은 시스템 상의 `공용 설정`에 이용하면 좋을 것 같다고 생각이 들었다.

### [예시 코드]

클래스 객체 생성시에 instance란 변수가 없으면 변수를 생성, 있으면 그냥 반환

```python
# test_singleton.py

class Singleton():
    def __new__(cls):
        # instance 존재 여부 확인
        if not hasattr(cls, 'instance'):
            cls.instance = super(Singleton, cls).__new__(cls)
        return cls.instance

if __name__ == '__main__':
    single_1 = Singleton()
    print(f'1번 세팅 : {single_1}')
    single_2 = Singleton()
    print(f'2번 세팅 : {single_2}')
```

### [실행 결과]

객체를 2번 생성시 최초 생성한 객체(1번)와 동일한 객체를 반환 받은 것을 확인할 수 있다.

![Result]({{ "/assets/img/post/2021-07-21-Designe-Pattern-Singleton/싱글턴-결과.png" | relative_url }})