# Plot tests

In pyleecan, each plot tests have to give the argument __is_show_fig__ to False when plotting. It allow to don't show the popup of the plot when the tests are running.
Here is an example :

```py
    # Plot, check and save
    machine.plot(is_show_fig=False)
```
