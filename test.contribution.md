Test contribution
=================

The only way to ensure that the implemented code is working and will
always do, is to develop validation tests. This page will present **how
to develop tests for PYLEECAN**.

We invite everyone contributing to the project to systematically add
tests to all their contributions if possible.

Test development guidelines
---------------------------

The python test package was initially based on
[unittest](https://docs.python.org/3/library/unittest.html#module-unittest),
but we decided to switch to [pytest](https://docs.pytest.org/en/latest/)
package for its ease of use. As pytest supports unittest, we still have
some unittest for the moment but **every new test should use pytest**.
To test a code, it is recommended to develop its specific test code for
each method and each class.

Install pytest
--------------

To install pytest, one can use the following command:

    python -m pip install -U pytest

Class tests
-----------

PYLEECAN classes are generated automatically, all the classes are built
in the same way. This fact enables us to test all the classes with one
test file which you can find in **Tests/Classes**. So you won\'t need to
develop tests for the class itself but you will have to develop the
tests for the methods defined in the [Class Creation](calss.generation.md).

All the methods tests are gathered in **Tests/Methods/\<subfolder\>**
with **Subfolder** the type of the class (*Geometry*, *Machine*, *Slot*,
etc.).

Validation tests
----------------

A validation test is a test running one or several simulations composed
of one or several modules to compare pyleecan results with other
software or publication results. The source of the comparison data must
be clearly referenced in the header of the file. A validation test can
also compare several way of computing the same quantity in pyleecan
(with/without symmetry, with different model...).

The machine and material for validation tests must be added in
pyleecan/Data. They are the reference machine and material that are
available as template in the GUI. When adding a new validation case, if
possible, use an existing validation machine/material. The more a
machine is used by different validation cases, the more the results can
be trusted.

A validation test must call the run method of Simulation objects and
compare (if possible) the resulting output with the out_list parameter
of the Output post-processing.

How to write a test with pytest
-------------------------------

With pytest, a test is a function located in file starting with "test_"
or ending with "_test". Test function name has to begin with "test_".

Here is a basic example of test:

``` {.python}
import pytest 

def test_upper():
    assert 'foo'.upper() == 'FOO'

def test_isupper():
    assert 'FOO'.isupper() 
    assert 'Foo'.isupper() == False

def test_split(self):
    s = 'hello world'
    assert s.split() == ['hello', 'world']

    # check that s.split fails when the separator is not a string
    with pytest.raises(TypeError):
        s.split(2)
```

How to run the tests
--------------------

To run the tests, you need to open a terminal, go into the folder which
contains PYLEECAN and execute the command: :

    python -m pytest

How to mark a test
------------------

Pytest enables to set metadata on the test functions with markers. This
feature enables to easily exclude or include some tests from the test
execution. Here is the list of some of the current markers used in
PYLEECAN:

-   validation : validation test, executes a workflow to check the results validity
-   long : test that last more than 30 seconds
-   FEMM : test using FEMM
-   GMSH : test using GMSH
-   DEAP : test using DEAP

The complete list is available in the file pyleecan/pytest.ini

The following command is an example to execute only validations test
that don\'t use FEMM: :

    python -m pytest -m "validation and not FEMM"

To mark a test, you just need to add it a decorator:

``` {.python}
import pytest 

@pytest.mark.validation
def test_upper():
    assert 'foo'.upper() == 'FOO'
```

It is also possible to set several markers to a test:

``` {.python}
import pytest 

@pytest.mark.long
@pytest.mark.FEMM
def test_lower():
    assert 'FOO'.lower() == 'foo'
```

Parametrizing test
------------------

Pytest enables to go much further and test more cases on a single test
by running a test with different input data. To do so, you just need to
use the *parametrize* marker. This marker has two arguments:

-   a tuple containing the test parameters names
-   a list containing tuples, each tuple contains the input data for one
    test run

One can also add markers to a specific input. In the following example
we use the *xfail* marker to specify that the test is supposed to fail
with (1, 0) in input:

``` {.python}
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

Which tests to develop
----------------------

An easy way to find a PYLEECAN part that needs to be tested is to use
[pytest-cov](https://github.com/pytest-dev/pytest-cov). This pytest
extension enables to see which lines in the code are not executed by the
existing tests. It can be installed with this command:

    python -m pip install -U pytest-cov

To run tests with coverage, use the following command:

    python -m pytest --cov --cov-report=html:report_folder

The report is located in *report_folder*. Then open the index.html
file:

![](_static/coverage_report.png)

**Within the report, you will see which files and which code lines are
not covered and find what to test next.**

![](_static/coverage.png)

**For example the Arc3 method discretize is not covered at 100%, there
are some lines not covered as line 40 and 42 colored in pink**

![](_static/coverage1.png)

In this case, there is no test to check that the discretization can
handle strange arguments.
