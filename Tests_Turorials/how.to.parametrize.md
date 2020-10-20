# Parametrizing test

Pytest enables to go much further and test more cases on a single test by running a test with different input data. To do so, you just need to use the parametrize marker. This marker has two arguments:
* a tuple containing the test parameters names
* a list containing tuples, each tuple contains the input data for one test run

One can also add markers to a specific input. In the following example we use the xfail marker to specify that the test is supposed to fail with (1, 0) in input:
```py
import pytest

@pytest.mark.parametrize(
    ("n", "expected"),
    [
        (1, 2),
        (4, 5),
        pytest.param(1, 0, marks=pytest.mark.xfail), # <-- The test is supposed to fail with this data
    ],
)
def test_increment(n, expected):
    assert n + 1 == expected
```

With this code, pytest will execute test_increment 3 times. First with n=1 & expected=2, second with n=4 & expected=5 and the last call. __Pytest.parm__ allow us to input
more value than expected to give more information to the test.

It is possible to set a list for the parametrize mark :

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
