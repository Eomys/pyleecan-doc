# How to mark a test

Pytest enables to set metadata on the test functions with markers. This feature enables to easily exclude or include some tests from the test execution. 

## How to use them ?

Here is an example :

```py
def multiply(x,y):
    return x * y
    
def lowerCase(text):
    return text.lower()


@pytest.mark.operation
def test_multiply():
    assert multiply(1,2) == 2

@pytest.mark.syntaxChange
def test_lowerCase():
    assert lowerCase("HELLO WORLD") == "hello world"
```

Here is two test methods which have different markers. To run the test with the __operation__ mark, the command line will be like :
```
python -m pytest -m "operation"
```
It will launch only test_multiply. It is possible to make the command more precise :
```
python -m pytest -m "operation and not syntaxChange"
```
With this idea you can specify the mark you don't want to launch :
```
python -m pytest -m "not operation"
```
It'll launch all tests except __mark.operation__ ones. Notice that it is possible to put multiple marks on a single test.


If the test methods are in a test class, you can also put markers for the class. But be carefull ! If you put a mark on a class, the mark will be on every test in the class.
Also, the mark on the class has the priority. It means that if you don't want to launch the class test, you won't be able to run test methods in the class whatever you do.


__Please notice__ : If you can make a test class and put mark on it, it will be more efficient than putting markers on each test methods. Because when pytest is running with
a mark specification, it'll search for all matching markers. It can slow the running of the tests.


## Markers in Pyleecan ?

Here is the list of some of the current markers used in PYLEECAN:

* validation : validation test, executes a workflow to check the results validity
* long : test that last more than 30 seconds
* FEMM : test using FEMM
* GMSH : test using GMSH
* etc...

The complete list is available in the file __pyleecan/pytest.ini__. If you want to create some, you'll have to write them in this file.

## Go Further

Pytest allow to parametrize tests. [Here is a another tutorial.](https://github.com/BenjaminGabet/pyleecan-doc/blob/patch-1/Tests_Turorials/how.to.parametrize.md)
