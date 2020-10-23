# Parametrizing test

Pytest enables to go much further and test more cases on a single test by running a test with different input data. This is a very good alternative to test different data without duplicating the code. With that, it is possible to have a set of data which will be right and one another false. The speed of the tests will be increase by doing parametrization ! To do so, just need to use the parametrize marker. This marker has two arguments:
* a tuple containing the test parameters names
* a list containing tuples, each tuple contains the input data for one test run

One can also add markers to a specific input. In the following example we use the xfail marker to specify that the test is supposed to fail with (1, 0) in input:
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

With this code, pytest will execute __test_is_on_line__ 2 times. First with the first element of the list named __is_on_line_list__ and the second element of the list. With this line __@pytest.mark.parametrize("test_dict", is_on_line_list)__ we can tell that the function __test_is_on_line__ will have in its parameters the list __is_on_line_list__ renamed by __test_dict__. And after that, the data can be used like a classic paramter. There is only one test function and it is possible to test it with plenty of different data to cover all the case.

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

## Go Further

Pytest allow us to make setup and teardown function with something called __fixture__. [Here is one more tutorial.](https://github.com/BenjaminGabet/pyleecan-doc/blob/patch-1/Tests_Turorials/make.setup.function.md)
