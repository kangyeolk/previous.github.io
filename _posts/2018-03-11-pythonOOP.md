---
title: 파이썬에서의 객체지향 프로그래밍
category: Some Concepts
tag: Some Concepts
---

## OOP(Object Oriented Programming) 방법

OOP는 코드의 재사용성 그리고 코드 관리 면에서 효율적인 프로그래밍 방법이다. class는 하나의 객체를 만드는 청사진이라고 할 수 있다 예를 들면 아파트 단지를 만드는 데 있어 같은 아파트를 10개 지어야한다고 하면 class는 같은 아파트를 찍어내는 설계도라고 할 수 있다. 그리고 class로 만들어 내은 객체를 **instance** 라고 한다. 예시로 돌아가면 아파트 한 동임 셈이다. 한편, 특정 class 내의 변수와 함수를 각각 **attribute** 와 **method** 라고 한다. 예시로 돌아가면 아파트의 방의 개수와 엘레베이터의 움직임이라고 할 수 있습니다.

## 파이썬에서의 OOP

파이썬에서 역시 OOP 방식의 코딩을 할 수 있습니다. 조금 다른 점은 파이썬은 모든 메소드에 해당 객체를 자동으로 넘기기 때문에 ```self```를 통해서 항상 그 객체를 받아주어야 된다는 것 입니다. 아래의 코드에서는 간단하게 파이썬에서의 class를 구현했습니다. ```def __init__(self, name, pay)```은 객체를 처음 만드는 **생성자** 입니다. 객체를 초기화 시킨다고 생각하면 됩니다.

```python
class Employee:

  raise_amount = 1.05

  def __init__(self, name, pay):
    self.name = name
    self.pay = pay

  def printPay(self):
    return '{}:{}'.format(self.name, self.pay)

  def raisePay(self):
    self.pay = int(self.pay * self.raise_amount)

sample = Employee('Tom', 4500)
# see object information
print(sample.__dict__)
```

### 인스턴스와 클래스

만들어진 인스턴스는 이제 class와 따로 떨어져서 독립적인 존재가 됩니다. 따라서 인스턴스를 통해서 수정을 하더라도 class 값 자체에 영향을 미치지는 않습니다. 하지만 class를 직접 수정하면 모든 인스턴스에 영향을 미칩니다. 즉, 인스턴스 변수를 수정하는 것과 클래스 변수를 직접 수정하는 것은 다른 영향을 줍니다.

```python
sample1 = Employee('Tom', 4500)
sample2 = Employee('Kim', 6000)

# instance variable modification
sample1.raise_amount = 1.08
print(sample1.raise_amount) # 1.08
print(sample2.raise_amount) # 1.05
sample1.raise_amount = 1.05

# class variable modification
Employee.raise_amount = 1.08
print(sample1.raise_amount) # 1.08
print(sample2.raise_amount) # 1.08
```

한편, 메소드 역시 클래스 메소드와 레귤러 메소드 그리고, 스태틱 메소드(static method)로 나눌 수 있습니다. ```@classmethod```는 메소드를 클래스 메소드로 만들어 줍니다. 이 메소드는 클래스 자체를 ```cls```인자로 받습니다. 그리고 인스턴스를 통해 메소드를 사용해도 클래스 전체에 영향을 미치게 됩니다. 레귤러 메소드는 앞에서 말한 것과 같이 인스턴스를 인자로 받습니다. 스태틱 메소드는 앞서 언급한 메소드들과 달리 인자로 아무것도 받지 않습니다.

```python
# continue to class Employee
  @classmethod
  def set_raise_amt(cls, amount):
    cls.raise_amount = amount

  @staticmethod
  def is_workday(day):
    if day.weekday() == 5 or if day.weekday() == 6:
      return False
    return True
```

### 파이썬에서의 상속

상속(inheritance)는 부모 클래스의 성질을 그대로 가져오면서 조금 변형할 수 있는 자식 클래스(subclass)를 만드는 것 입니다. 자식 클래스는 기본적으로 부모 클래스의 변수 혹은 메소드를 모두 가지고 있습니다. 하지만 부모 클래스에서 받은 변수나 메소드를 중복(overwriting)해서 재정의 한다면 자식 클래스의 정의를 우선하여 정해지게 됩니다.

```python
class Developer(Employee):
  raise_amount = 1.1

  def __init__(self, name, pay, prog_lang):
      # self.name = name, self.pay = pay 대신 다음과 같이 부모의 초기화를 그대로 할 수 있다.
      super().__init__(name, pay)
      self.prog_lang = prog_lang

dev1 = Developer('Mark', 3000)
dev2 = Developer('Wang', 4000)
print(help(dev1)) # see structure of object

print(dev1.pay) # 3000
dev1.raisePay()
print(dev1.pay) # 3300

class Manager(Employee):

  def __init__(self, name, pay, employees=None):
      super().__init__(name, pay)
      if employees is None:
        self.employees = []
      else:
        self.employees = employees

  def add_emp(self, emp):
    if emp not in self.employees:
      self.employees.append(emp)

  def remove_emp(self, emp):
    if emp in self.employees:
      self.employees.remove(emp)

  def print_emps(self):
    for emp in self.employees:
      print(emp.name)

mgr = Manager('Soo', 90000, [dev1])
mgr.print_emps() # Mark

mgr.add_emp(dev2)
mgr.remove_emp(dev1)
mgr.print_emps() # Wang
```

### ```is.instance``` & ```is.subclass```

파이썬에는 인스턴스와 클래스 간의 관계 혹은 클래스사이의 관계를 알아보기 위한 함수인 ```is.instance```와 ```is.subclass```가 존재합니다.

```python
print(isinstance(dev1, Developer)) # True
print(isinstance(dev1, Manager)) # False

print(issubclass(Manager, Employee)) # True
print(issubclass(Manager, Developer)) # False
```

### 특별한 메소드

파이썬 class는 특정한 역할을 하는 특별한 메소드를 가집니다. ```__repr__``` 와 ```__str__```은 모두 문자열을 리턴해 줍니다. 그 외에도 클래스에서는 특별한 메소드를 많이 가지고 있습니다.

```python
# continue to Employee
  def __repr__(self):
    return 'pay of {} is {}'.format(self.name, self.pay)

  def __str__(self):
    return '{} - {}'.format(self.name, self.pay)

emp = Employee('Thomas', 20000)
print(repr(emp)) # can use this kind of grammer  
```

### Getter, Setter, Deleter

```python
class adding:

  def __init__(self, first, second):
    self.first = first
    self.second = second
    # self.added = '{} {}'.format(self.first, self.second) -(1)

    @property # -(2)
    def mAdded(self):
      return '{} {}'.format(self.first, self.second)


    @mAdded.setter
    def mAdded(self, com):
      first, second = com.split(' ')
      self.first = first
      self.second = second

    @mAdded.deleter
    def mAdded(self):
      self.first = None
      self.second = None

temp = adding('one', 'two')
temp.first = 'three'
#print(temp.first) # three
#print(temp.added) # onetwo
print(temp.mAdded) # threetwo

# setter, deleter의 활용
temp.mAdded = '1 2' # setting self.first & self.second
del temp.mAdded
```

위와 같이 (1) 경우에는 초기화 때 값이 정해지기 때문에 ```first```가 변하더라도 ```added```가 업데이트되지 않는 문제가 있습니다. 이를 보완하기 위해서 (2)와 같은 방법을 사용합니다. ```@property```를 붙여주면 메소드를 변수와 같이 사용할 수 있습니다. 또한 그 메소드에 대해서 setter 혹은 deleter를 통해서 다른 작업 역시 할 수 있습니다.
