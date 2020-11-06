# How to launch some specific tests but not all in a class/file/folder?

Pytest enables to set metadata on the test functions with markers. This feature enables to easily exclude or include some tests from the test execution. 

## How to use them?

Here is an example:

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

Here there are two test methods which have different markers. To run the test with the __operation__ mark, the command line will be:
```
python -m pytest -m "operation"
```
It is also possible to specify which mark the test should not launch:
```
python -m pytest -m "not operation"
```
This will launch all tests except __mark.operation__ ones. Finally, several markers can be called in a single test:
```
python -m pytest -m "operation and not syntaxChange"
```

If the test methods are in a test class, one can also put markers for the class. But be carefull! If one put a mark on a class, the mark will be applied on every test in the class.
Also, the mark on the class has the priority. It means that if someone doesn't want to launch the class test, he won't be able to run test methods in the class whatever he does.

```py
def multiply(x,y):
    return x * y

@pytest.mark.long
class Test_Operation(object):

    @pytest.mark.http
    def test_multiply_http(self):
        assert multiply(3, 3) == 9

    def test_multiply(self):
        assert multiply(3, 3) == 9

```
If the test is launched with this command:
```
pytest -m "http and not long"
```
No test will be executed because the class mark has the priority.

__Please note__: Putting a marker on a test class is more efficient than putting markers on each test methods, because when pytest is running with
a mark specification, it will search for all matching markers, which can slow the running of the tests if there are many of them.


## Markers in Pyleecan?

Here is the list of some of the current markers used in PYLEECAN:

* validation : validation test, executes a workflow to check the results validity
* long : test that last more than 30 seconds
* FEMM : test using FEMM
* GMSH : test using GMSH
* etc...

The complete list is available in the file __pyleecan/pytest.ini__. To create a new marker, please add it in this file.

__Please note__: To do a rapid check of the tests in pyleccan, the "long" marker should be excluded when the test are running:

```
pytest -m "not long"
```

## To go Further

Pytest allows to parametrize tests. [Here is a another tutorial.](https://github.com/Eomys/pyleecan-doc/blob/master/Tests_Turorials/how.to.parametrize.md)
