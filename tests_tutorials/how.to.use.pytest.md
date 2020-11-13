# How to write a test with pytest

The python test package was initially based on unittest, but it has been decided to switch to pytest package for its ease of use. Every new test should use pytest. 
To test a code, it is recommended to develop its specific test code for each method and each class.

## How to install pytest?

To install pytest, one can use the following command:
```
pip install -U pytest
```
After that, pytest will be installed and ready to be used.

## How to write a test with pytest?

Here is an example:
 
 ```py
def multiply(x, y):
    return x * y


def test_multiply():
    assert multiply(3, 3) == 9            #True
```

To test a simple method without a class, it is possible to write the test function like above. 
Please notice that with pytest, a test is a function located in file starting with “test_” or ending with “test”. Test function names have to begin with “test” in Pyleecan.

Sometimes, it will be necessary to create a test class to make files cleaner or because it cannot work without it. Here is how to do it:

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

The class test is always beginning with __Test___. In the test_divide, the method is dividing 3 by 0 and pytest is raising the error __ZeroDivisionError__. It is quite
usefull to test errors.

## How to run the tests?

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
Functions and GUI folder can be tested using the following commands:
```
pytest ./Tests/GUI
pytest ./Tests/Functions
```
It is possible to combine -m, -v like this :
```
pytest -v -m "GUI" ./Tests/Methods/Slot/test_HoleM50_meth.py
```
## Some useful function of pytest

Some functions that are important to know:
```
with pytest.raises(ZeroDivisionError):
    something_that_will_cause_the_error :    1/0
    
assert 0.1 + 0.2 == 0.3                     ----> False  due to that :  https://docs.python.org/3/tutorial/floatingpoint.html
assert 0.1 + 0.2 == pytest.approx(0.3)      ----> True

Both the relative and absolute tolerances can be changed by passing arguments to the approx constructor:
1 + 1e-8 == approx(1)                       ----> True
1 + 1e-8 == approx(1, abs=1e-12)            ----> False
1 + 1e-8 == approx(1, rel=1e-6, abs=1e-12)  ----> True
If you specify abs but not rel, the comparison will not consider the relative tolerance at all. In other words, two numbers that are within the default relative tolerance of 1e-6 will still be considered unequal if they exceed the specified absolute tolerance. If you specify both abs and rel, the numbers will be considered equal if either tolerance is met.


numpy.testing.assert_array_almost_equal([1.0,2.333],[1.0,2.333])               ----> True
np.testing.assert_array_almost_equal([1.0,2.33333],[1.0,2.33339], decimal=5)   ----> False    2.33333 != 2.33339
```
pytest-mock can also be very handy. Have a look at [this tutorial](https://changhsinlee.com/pytest-mock/) for more details.

## To go Further

Pytest allows to put markers on tests. [Here is a tutorial for how to use markers in PYLEECAN.](how.to.use.markers.md)
