# Parametrizing test

Pytest enables to go much further and test more cases on a single test by running a test with different input data. This is a very good alternative to test different data without duplicating the code. With this method, it is possible to have a set of data for which the test should pass and another one for which it should fail. The speed of the tests will also be increased by using parametrization. To do so, the parametrize marker simply needs to be used. This marker has two arguments:
* the name of the parameter to be used in the test;
* the parameter which can be a dict, a list, a simple value or everything.

Here is an example with a dict:

```py
import pytest

"""Tests of the function is_on_line from Arc meth"""
ref_dict = {
    "arc": Arc1(begin=-2j, end=-1 - 1j, radius=-1, is_trigo_direction=False),
    "Z": -1 - 2j,  # First point of cutting line
    "result": False,
}

@pytest.mark.parametrize("test_dict", ref_dict)
def test_is_on_line(test_dict):
    """Check is_on_line method"""
    arc_obj = test_dict["arc"]

    result = arc_obj.is_on_line(test_dict["Z"])

    assert result == test_dict["result"]
```


Here is an example with a list containing two dict:

```py
import pytest

"""Tests of the function is_on_line from Arc meth"""

is_on_line_list = list()

# 1  Check not on the circle
is_on_line_list.append(
    {
        "arc": Arc1(begin=-2j, end=-1 - 1j, radius=-1, is_trigo_direction=False),
        "Z": -1 - 2j,  # First point of cutting line
        "result": False,
    }
)

# 2  Check on the circle
is_on_line_list.append(
    {
        "arc": Arc1(begin=-2j, end=-1 - 1j, radius=-1, is_trigo_direction=False),
        "Z": -1j + exp(1j * 5 * pi / 4),  # First point of cutting line
        "result": True,
    }
)

@pytest.mark.parametrize("test_dict", is_on_line_list)
def test_is_on_line(test_dict):
    """Check is_on_line method"""
    arc_obj = test_dict["arc"]

    result = arc_obj.is_on_line(test_dict["Z"])

    assert result == test_dict["result"]
```

With this code, pytest will execute __test_is_on_line__ 2 times. First with the first element of the list named __is_on_line_list__ and then with the second element of the list. With this line __@pytest.mark.parametrize("test_dict", is_on_line_list)__ we can tell that the function __test_is_on_line__ will have in its parameters the list __is_on_line_list__ renamed by __test_dict__. And after that, the data can be used like a classic parameter. There is only one test function and it is possible to test it with many different data to cover all the cases. This second syntax with a list of dict is the standard currently used in pyleecan. 

Also, it is possible to set a list for the parametrize mark :

```py
import pytest

def multiply(x, y):
    return x * y

lst = ((1,2,2),(2,2,4),(50,10,500))

@pytest.mark.parametrize(
      ('x','y','result'),
      lst
)
def test_multiply(x, y, result):
    assert multiply(x,y) == result
```

## To go Further

Pytest allows us to make setup and teardown function with something called __fixture__. [Here is one more tutorial.](https://github.com/Eomys/pyleecan-doc/blob/master/Tests_Turorials/make.setup.function.md)
