# How to make a setup function with Pytest

To make a setup function, Pytest provide fixture functions. It's just a simple function that we launch before every test functions in the test class. This method allow to make a common setup funcion for all the tests in the file/class. This is very useful. It can also be a
function we launch before only one test not in a class. Here is an example :

```py

def multiply(x, y):
    return x * y
    
@pytest.fixture
def setup():
    
    # SETUP
    
    lst = []
    for i in range(10):
        lst.append((i, i*2, i*i*2))

    yield lst
    
    # TEARDOWN
    
    lst = []

def test_multiply(setup):                           #setup = lst
    for i in setup:
        assert multiply(i[0],i[1]) == i[2]
        
def test_reverse_lst(setup):
    setup.reverse()
    assert setup[0][2] == 162    #(9*9*2)

def test_lst(setup):
    assert setup[0][2] == 0      #(0*0*2)
```

The first method is multiplying x by y. Then the test_multiply function has __setup__ in his parameter. It means that the test function is calling the fixture one specified by
__@pytest.fixture__. The function named __setup__ is launched just before test_multiply to load the data we need and return it with __yield__. It could have been __return__ 
but return doesn't allow us to continue the function after it happens. When pytest reach yield, it gives the data to test_multiply and the test can go forward. Once it have
finished, the setup function continue after the yield, it becomes now a __teardown__ function.

A fixture allow us to setup some data before a test function process and after it has done its work. After the yield we can free some memory or close an application etc...

## Go Further

Want to contribute ? [Check those guidelines](https://github.com/BenjaminGabet/pyleecan-doc/blob/patch-1/Tests_Turorials/how.to.contribute.md)
