# How to make a setup function with Pytest

To make a setup function, Pytest provide fixture functions. It's just a simple function that we launch before every test functions in the test class. It can also be a
function we launch before only one test not in a class. Here is an example :

```py

def multiply(x, y):
    return x * y
    
@pytest.fixture
def setup():
    lst = []
    for i in range(10):
        lst.append((i, i*2, i*i*2))

    yield lst

    lst = []

def test_multiply(setup):
    for i in lst:
        assert multiply(i[0],i[1]) == i[2]
```

The first method is multiplying x by y. Then the test_multiply function has __setup__ in his parameter. It means that the test function is calling the fixture one specified by
__@pytest.fixture__. The function named __setup__ is launched just before test_multiply to load the data we need and return it with __yield__. It could have been __return__ 
but return doesn't allow us to continue the function after it happens. When pytest reach yield, it gives the data to test_multiply and the test can go forward. Once it have
finished, the setup function continue after the yield, it becomes now a __teardown__ function.

A fixture allow us to setup some data before a test function process and after it has done its work. After the yield we can free some memory or close an application etc...

## Go Further

If you want to contribute : [check those guidelines](https://github.com/BenjaminGabet/pyleecan-doc/blob/patch-1/Tests_Turorials/how.to.contribute.md)
