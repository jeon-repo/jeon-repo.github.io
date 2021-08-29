---
layout: post
title: "[Design Pattern] 프록시 패턴 (Proxy)"
color: black
author: janos
tags: [디자인패턴]
---

---

### 설명

위키백과에 나온 내용은 다음과 같습니다.

> 일반적으로 프록시는 다른 무언가와 이어지는 인터페이스의 역할을 하는 클래스이다. 프록시는 어떠한 것(이를테면 네트워크 연결, 메모리 안의 커다란 객체, 파일, 복제할 수 없거나 수요가 많은 리소스)과도 인터페이스의 역할을 수행할 수 있다.
> 
> 복합적인 오브젝트들의 다수의 복사본이 존재해야만 하는 상황에서 프록시 패턴은 애플리케이션의 메모리 사용량을 줄이기 위해서 `플라이웨이트 패턴` 과 결합된 형태로 나올 수도 있다. (`GoF 구조`에 포함)
> 
> "위키백과 - [Proxy](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9D%EC%8B%9C_%ED%8C%A8%ED%84%B4)"

- 복잡한 시스템을 간단하게 나타낼 때 사용한다고 합니다.
- 실제 오브젝트에 보안을 제공할 수 있다고 합니다.
- 유튜브에서 **마우스**가 영상에 `포커싱` 되기 전에는 `클립 이미지` 를 보여주고, `포커싱` 이 되었을 때 `클립 영상` 을 보여주는 상황에 이용하면 좋을 것 같다고 생각이 들었습니다.

### 예시 코드

client가 영상에 `포커싱`을 하기 전에는 `클립 이미지` 를 보여주고, `포커싱` 되었을 때 `클립 영상`이 재생

(SubInterface의 __ metaclass__ = ABCMeta 설정은 인터페이스의 역할)

```python
import abc

class SubInterface:
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def show(self):
        pass

class Video(SubInterface):
    def show(self):
        return '클립 영상 재생'

class Proxy(SubInterface):
    def __init__(self, video):
        self.video = video

    def show(self, state):
        if state == 'focusing':
            return self.video.show()
        else:
            return '클립 이미지 상태'

class Client:
    def __init__(self, _object):
        self.state = None
        self.object = _object

    def move_mouse(self):
        self.state = 'focusing'

    def show(self):
        return self.object.show(self.state)

if __name__ == '__main__':
    origin_video = Video()
    proxy = Proxy(origin_video)
    client = Client(proxy)
    
    print(f'아무것도 안함 : {client.show()}')
    client.move_mouse()
    print(f'영상에 포커싱함 : {client.show()}')
```

### 결과

클립 이미지를 보여주다가 영상에 마우스가 포커싱 될 때, 클립 영상이 재생된 것을 확인할 수 있습니다.

![Result]({{ "/assets/img/post/2021-08-06-Designe-Pattern-Proxy/프록시-결과.png" | relative_url }})