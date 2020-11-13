# How to make a setup function with Pytest

To make a setup function, Pytest provides fixture functions. A fixture function is a simple function that is launched before every test functions in the test class. This method allows to make a common setup function for all the tests in the file/class, which can be very useful. It can also be a function which is launched before a single test, outside of a class. Here is an example :

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

The first method is multiplying x by y. Then the test_multiply function has __setup__ in his parameter. It means that the test function is calling the fixture specified by
__@pytest.fixture__. The function named __setup__ is launched just before test_multiply to load the data we need and return it with __yield__. It could have been __return__ 
but return doesn't allow us to continue the function after it is called. When pytest reach yield, it gives the data to test_multiply and the test can go forward. Once it is
finished, the setup function continues after the yield, it becomes now a __teardown__ function.

A fixture allows us to setup some data before a test function process and after it has done its work. After the yield, we can free some memory, or close an application, etc...

## To go Further

Want to contribute? [Check those guidelines.](how.to.contribute.md)
