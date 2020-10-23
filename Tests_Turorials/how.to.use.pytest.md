# How to write a test with pytest

The python test package was initially based on unittest, but it have been decided to switch to pytest package for its ease of use. Every new test should use pytest. 
To test a code, it is recommended to develop its specific test code for each method and each class.

## How to install pytest ?

To install pytest, one can use the following command:
```
pip install -U pytest
```
After that, pytest will be installed and ready to be used.

## How to write a test with pytest

Here is an example :
 
 ```py
def multiply(x, y):
    return x * y


def test_multiply():
    assert multiply(3, 3) == 9            #True
```

To test a simple method without a class, it is possible to write the test function like above. 
Please notice that with pytest, a test is a function located in file starting with “test_” or ending with “test”. Test function name has to begin with “test” in Pyleecan.

Sometimes, It'll be necessary to create a test class to make files cleaner or because it can't work without it. Here is how to do :

```py
class Operation(object):

    def multiply(self, x, y):
        return x * y

    def add(self, x, y):
        return x + y
    
    def divide(self, x, y):
        return x/y


class Test_operation(object):

    def test_multipy(self):
        op = Operation()
        assert op.multiply(3, 3) == 9        #True

    def test_add(self):
        op = Operation()
        assert op.add(3, 3) == 6        #True

    def test_divide(self):
        op = Operation()
        # check that divided by 0 fails and return an error
        with pytest.raises(ZeroDivisionError):
            op.divide(3, 0)

```

The class test is always beginning with __Test___. In the test_divide, the method is dividing 3 by 0 and pytest is raising the error __ZeroDivisionError__. It's quite
usefull to test errors.

## How to run the tests ?

To run the tests, open a terminal, go into the folder which contains PYLEECAN and execute the command:
```
python -m pytest
```
It is possible to launch a test of a precise folder or all the tests in a folder. To do it:
```
python -m pytest ./<Folder>/<SubFolder>/.../<File>.py
or
python -m pytest ./<Folder>
```
If the input of the tests does not give enough information, verbose can be add by adding __-v__ like this:
```
pytest -v ...
```
Functions and GUI folder can be test by doing that in pyleecan :
```
pytest ./Tests/GUI
pytest ./Tests/Functions
```

## Go Further

Pytest allow to put markers on tests. [Here is a tutorial for Pyleecan.](https://github.com/BenjaminGabet/pyleecan-doc/blob/patch-1/Tests_Turorials/how.to.use.marks.md)
