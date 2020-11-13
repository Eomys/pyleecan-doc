# How to contribute to test development

## Where to create a test file

PYLEECAN classes are generated automatically, all the classes are built in the same way. 
This fact enables us to test all the classes with one test file which can be find in Tests/Classes. 
It's still usefull to test your classes and its methods in case of some issue appears after the automatic generator process.

All the methods tests are gathered in Tests/Methods/\<subfolder> with Subfolder the type of the class (Geometry, Machine, Slot, etc.). And everything goes like that.
In the Tests directory, there are some various subdirectories:
__Classes__ is for tests concerning the class themselves (making sure that the class generator return a correct class)
__Functions__ is for tests on the generic function that can be found in the function folder of pyleecan
__GUI__ is for all the tests regarding the GUI
__Methods__ is for checking that each method of each class behave as it should. 
__Plot__ is to gather the plot methods test with various data or topologies
__Tutorials__ is to include the tutorial and webinar notebook into the tests
__Validation__ gather the tests that running one or several simulations to compare pyleecan with other software or publication results

## Which tests to develop

As always, if you want to contribute to the validation of pyleecan, start by [opening an issue to talk](https://github.com/Eomys/pyleecan/issues) about it. 
An easy way to find a PYLEECAN part that needs to be tested is to use [coveragepy](https://github.com/nedbat/coveragepy/blob/coverage-5.3/doc/index.rst). This pytest extension enables to see which lines in the code are not executed by the existing tests. It can be installed with this command:
```
pip install coverage
```
To launch test:
```
pytest arg1 arg2 arg3
```
Then launch tests under coverage with:
```
coverage run -m pytest arg1 arg2 arg3

coverage run -m pytest ./Tests
coverage run -m pytest -v ./Tests/GUI
coverage run -m pytest -v -m "not long"
```
Once tests have been executed, the report of the coverage is accessible by doing those two commands:
```
coverage report -m
coverage html
```
The first one will allow to print the report in the computer prompt, the second one will create a file html for each files. If index.html is opened, then this should appear:

![img](https://pyleecan.org/_images/coverage_report.png)

Within the report, it'll show which files and which code lines are not covered and find what to test next.
For example the Arc3 method discretize is not covered at 100%, there are some lines not covered as line 40 and 42 colored in pink :

![img](https://pyleecan.org/_images/coverage1.png)

In this case, there is no test to check that the discretization can handle strange arguments.

__Please notice__ : A file at 100% of coverage isn't a file without bugs. The coverage show just which lines are tested or not !

## How to make a good formated code

First of all, all variables should be named correctly by describing what they are doing. The code should be commented to.

Also, there is a package which enhance the code by formatting it. It's called Black : [Feel free to check the documentation](https://black.readthedocs.io/en/stable/)

Install it and run it by using those commands:
```
pip install -Iv black==20.8b1
python -m black {source_file_or_directory}
```

## How to contribute to the documentation

There are two different ways to contribute to documentation:
* __By adding docstrings in python modules to document a specific segment of code__. For example the function square below, with docstrings in the triple quotes.
```py
def square(param1):

    """Return the square of param1

    Parameters
    ----------
    param1 : type of the parameter (int)
            description of the parameter
    Returns
    -------
    res type of the value returned (int)
        description of the value returned
    """
    res =  param1 ** 2
    return res
```

In this case one needs to commit his modifications and make a pull request to 
[submit his contribution](https://pyleecan.org/code.contribution.html) to the [pyleecan](https://github.com/Eomys/pyleecan) code repository. 
Once his modifications merged, the doc will be regenerated and update the website ourselves.
* By adding (or correcting) a .md file, for instance
  * if one has developed a new feature and he want to make a tutorial, he can add new “.md” files. [Check this documentation](https://agea.github.io/tutorial.md/)
  * if one has found some errors (typos) in the documentation and he wants to correct it. In this case he can correct them on [pyleecan-doc](https://github.com/Eomys/pyleecan-doc) repository by finding and correcting the corresponding “md” file. He can do that directly through Github (it will automatically create a fork of the repository in his github account).
  
  Then he should again submits his contribution on pyleecan-doc repository to share with the community. 
  After his modifications merged and the html pages regenerated, the submitted documentation will be available on the PYLEECAN website.
  
[For more information about the documentation.](https://pyleecan.org/doc.contribution.html)

## To go Further

In pyleecan, we can also test the GUI. [Here is a another tutorial.](https://github.com/Eomys/pyleecan-doc/blob/master/Tests_Turorials/how.to.unit.test.GUI.md)
