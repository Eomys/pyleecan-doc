# Plot tests

In pyleecan, each plot test has to give the argument __is_show_fig__ to False when plotting. That way, the popup of the plot won't open when running the tests.
Here is an example :

```py
    # Plot, check and save
    machine.plot(is_show_fig=False)
```
