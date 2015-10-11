# How to create python package
Animals라는 이름의 패키지를 만든다고 하자. 이 패키지는 Mammals와 Birds라는 두 모듈을 가지고 있다. 각각의 모듈은 동명의 클래스를 갖고 있다.

## Step 1: Create the Package Directory
일단 Animals라는 디렉토리를 만든다.

## Step 2: Add Classes
이제, 각 모듈에 두 클래스를 만들자. 우선 Mammals.py에 다음과 같이 넣는다:
```python
class Mammals:
    def __init__(self):
        ''' Constructor for this class. '''
        # Create some member animals
        self.members = ['Tiger', 'Elephant', 'Wild Cat']
 
 
    def printMembers(self):
        print('Printing members of the Mammals class')
        for member in self.members:
            print('\t%s ' % member)
```

그 다음, Birds에도 같은 방식으로 클래스를 만든다:
```python
class Birds:
    def __init__(self):
        ''' Constructor for this class. '''
        # Create some member animals
        self.members = ['Sparrow', 'Robin', 'Duck']
 
 
    def printMembers(self):
        print('Printing members of the Birds class')
        for member in self.members:
           print('\t%s ' % member)
```

## Step 3: Add the \_\_init\_\_.py File

마지막으로, Animals 디렉토리에 \_\_init\_\_.py를 만들고 다음과 같이 넣어준다:
```python
from Mammals import Mammals
from Birds import Birds
```
\_\_init\_\_.py가 필요한 이유는 Python이 이 파일을 통해  해당 디렉토리가 Python 패키지인지 일반 디렉토리인지 파악할 수 있기 때문이다. 이 때, \_\_init\_\_.py에서 import해놓은 클래스들만  외부 python 파일이 import할 수 있다.

Animals 디렉토리가 있는 지점에 test.py를 만들어 다음과 같이 테스트해볼 수 있다:
```python
# Import classes from your brand new package
from Animals import Mammals
from Animals import Birds
 
# Create an object of Mammals class & call a method of it
myMammal = Mammals()
myMammal.printMembers()
 
# Create an object of Birds class & call a method of it
myBird = Birds()
myBird.printMembers()
```

## References
<http://pythoncentral.io/how-to-create-a-python-package/>