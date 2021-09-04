---
layout: post
title: "[Python] 컴프리헨션(Comprehention)"
feature-img: "assets/img/feature-img/circuit.jpeg"
author: janos
tags: [Python]
---

---

## 컴프리헨션이란?

- Python에서 컴프리헨션(Comprehension)이란 list, dictionary, set의 `자료구조를 보다 쉽게 저장, 검색을 할 수 있게 하는 문법`입니다.
- 컴프리헨션은 list, dictionary, set의 내부에 코드를 작성하는 특징이 있습니다.

> ⚠️ 컴프리헨션은 입력 시퀀스에 있는 값 별로 객체를 담은 자료구조를 통째로 생성하기 때문에 메모리가 많이 소모될 수 있습니다.  
→ 하지만 이런 문제를 해결하기 위해서 Python에서는 `제너레이터` 표현식을 사용합니다.

---

## 컴프리헨션 표현식 형태

```
List       : [표현식 for 객체 in 순회가능객체] + 조건, 다중 반복문
Dictionary : {키_표현식 : 값_표현식 for 객체 in 순회가능객체} + 조건, 다중 반복문
Set        : {표현식 for 객체 in 순회가능객체} + 조건, 다중 반복문
```

- **리스트 컴프리헨션 (List Comprehension)**

	```python
	var_list = []
	
	for i in range(5):
		var_list.append(i+1)
	
	print(var_list)
	---------------------------------------------------
	[1, 2, 3, 4, 5]
	```

	위의 코드는 반복문을 통해 1 ~ 5의 숫자를 갖는 list를 생성하는 코드입니다.  
	아래는 위의 코드와 동일하게 list를 생성하는 컴프리헨션입니다.

	```python
	var_list = [i+1 for i in range(5)]
	
	print(var_list)
	---------------------------------------------------
	[1, 2, 3, 4, 5]
	```

- **딕셔너리 컴프리헨션 (Dictionary Comprehention)**

	```python
	students = ['짱구', '철수', '유리', '훈이', '맹구']
	var_dict = {}
	
	for name in students:
		var_dict[name] = 'human'
	
	print(var_dict)
	---------------------------------------------------
	{'짱구': 'human', '철수': 'human', '유리': 'human', '훈이': 'human', '맹구': 'human'}
	```

	위의 코드는 students의 학생들을 `key` 로 'human'을 `value` 를 할당하는 dictionary를 생성하는 코드입니다.  
	아래는 위의 코드와 동일하게 dictionary를 생성하는 컴프리헨션입니다.

	```python
	students = ['짱구', '철수', '유리', '훈이', '맹구']
	var_dict = {name: 'human' for name in students}
	
	print(var_dict)
	---------------------------------------------------
	{'짱구': 'human', '철수': 'human', '유리': 'human', '훈이': 'human', '맹구': 'human'}
	```

- 셋 컴프리헨션 (Set Comprehention)

	```python
	var_set = set()
	
	for i in range(5):
		var_set.add(i+1)
	
	print(var_set)
	---------------------------------------------------
	{3, 2, 1, 5, 4}
	```

	위의 코드는 반복문을 통해 1 ~ 5의 숫자를 갖는 set(집합)을 생성하는 코드입니다.  
	set(집합)은 괄호의 형태가 dictionary와 동일하기 때문에 `{}` 를 사용하지 않고 `set()` 을 사용하여 선언합니다.  
	아래는 위의 코드와 동일하게 set(집합)을 생성하는 컴프리헨션입니다.

	```python
	var_set = {i+1 for i in range(5)}
	
	print(var_set)
	---------------------------------------------------
	# set은 데이터의 순서를 구분하지 않아서 출력시 순서가 일정하지 않다.
	{2, 5, 3, 1, 4}
	```