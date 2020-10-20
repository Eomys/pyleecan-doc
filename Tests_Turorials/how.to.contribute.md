# How to contribute to test development

## Where to create a test file

PYLEECAN classes are generated automatically, all the classes are built in the same way. 
This fact enables us to test all the classes with one test file which you can find in Tests/Classes. 
So you won't need to develop tests for the class itself but you will have to develop the tests for the methods defined in the Class Creation.

All the methods tests are gathered in Tests/Methods/\<subfolder> with Subfolder the type of the class (Geometry, Machine, Slot, etc.). And everything goes like that.
In the Tests directory, you can find some various subdirectories. 
If you're writing a test which it concerns the GUI, you'll have to put your test in the subdirectory GUI. 
In a same way, if you're writing a test which it concerns a Plot, you'll have to put your test in the subdirectory Plot.

## Which tests to develop

An easy way to find a PYLEECAN part that needs to be tested is to use [coveragepy](https://github.com/nedbat/coveragepy/blob/coverage-5.3/doc/index.rst). This pytest extension enables to see which lines in the code are not executed by the existing tests. It can be installed with this command:
```
pip install coverage
```
If you usually use:
```
pytest arg1 arg2 arg3
```
Then you can run your tests under coverage with:
```
coverage run -m pytest arg1 arg2 arg3
```
Once your tests have been executed, you can start the report of the coverage by doing those two commands:
```
coverage report -m
coverage html
```
The first one will allow you to print the report in your computer prompt, the second one will create a file html for each files. If you open the index.html, you should see:

![img](https://pyleecan.org/_images/coverage_report.png)

Within the report, you will see which files and which code lines are not covered and find what to test next.
For example the Arc3 method discretize is not covered at 100%, there are some lines not covered as line 40 and 42 colored in pink :

![img](https://pyleecan.org/_images/coverage1.png)

In this case, there is no test to check that the discretization can handle strange arguments.

## How to make a good formated code

First of all, you have to name your variables correctly by describing what they are doing. You can make it better by commenting your code.

Also, there is a package which is allowing you to enhance your code by formatting it. It's called Black : [Feel free to check the documentation](https://black.readthedocs.io/en/stable/)

You can install and run it by using those commands:
```
pip install black
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

In this case you then need to commit your modifications and make a pull request to 
[submit your contribution](https://pyleecan.org/code.contribution.html) to the [pyleecan](https://github.com/Eomys/pyleecan) code repository. 
Once your modifications merged, we will regenerate the doc and update the website ourselves.
* By adding (or correcting) a .md file, for instance
  * if you have developed a new feature and you want to make a tutorial, you can add new “.md” files. [Check this documentation](https://agea.github.io/tutorial.md/)
  * if you have found some errors (typos) in the documentation and you want to correct it. In this case you can correct them on [pyleecan-doc](https://github.com/Eomys/pyleecan-doc) repository by finding and correcting the corresponding “md” file. You can do that directly through Github (it will automatically create a fork of the repository in your github account).
  
  Then you should again submit your contribution on pyleecan-doc repository to share with the community. 
  After your modifications merged and the html pages regenerated, the submitted documentation will be available on the PYLEECAN website.
  
[For more information about the documentation.](https://pyleecan.org/doc.contribution.html)
