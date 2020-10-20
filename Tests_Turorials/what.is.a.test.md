# What is a test and why testing ?

The only way to ensure that the implemented code is working and will always do, is to develop validation tests.
A validation test can be written for a function or a class. Most of the time, they are testing if the result of a function is the one we should obtain after
putting definite values to parameter of the function. Here is a example :

```py
def multiply(x, y):
    return x * y


def test_multiply():
    assert multiply(3, 3) == 9            #True
    assert multiply(4, 9) >= 36           #True
    assert multiply(10, 3) < 30           #False (And will cause an error that stops your program)
    assert not multiply(10, 3) < 30       #True
```

__Assert__ is the word to test something. In this example we are testing the multiply function which is multiplying x by y. We are testing four times the function 
with different values to ensure that is working. One of them is false and will cause an error. In reality, a test is not supposed to be wrong.
If it happens, either the function is wrong, or the test itself.

We invite everyone contributing to the project to systematically add tests to all their contributions if possible. 
You'll learn how to use pytest more precisely [here](https://github.com/BenjaminGabet/pyleecan-doc/blob/patch-1/Tests_Turorials/how.to.use.pytest.md) !

## Validation tests

A validation test is a test running one or several simulations composed of one or several modules to compare pyleecan results with other software or publication results. The source of the comparison data must be clearly referenced in the header of the file. A validation test can also compare several way of computing the same quantity in pyleecan (with/without symmetry, with different model…).

The machine and material for validation tests must be added in pyleecan/Data. They are the reference machine and material that are available as template in the GUI. When adding a new validation case, if possible, use an existing validation machine/material. The more a machine is used by different validation cases, the more the results can be trusted.

A validation test must call the run method of Simulation objects and compare (if possible) the resulting output with the out_list parameter of the Output post-processing.
