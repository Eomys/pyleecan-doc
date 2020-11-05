# Debugging with pytest

Debugging allow us to print variable during the execution of the test. If the test is false and the reason why is not obvious, debugging is a solution to find where is the problem.

To debug with pytest, we are using this line : 
```py
pytest.set_trace()
```

Here is an example in __pyleecan\Tests\Methods\Slot\test_HoleM50_meth.py__:

```py
    def test_build_geometry_two_hole_with_magnet(self):
        """check that curve_list is correct (one hole)"""
        test_obj = LamHole(is_internal=True, is_stator=False, Rext=0.075)
        test_obj.hole = list()
        test_obj.hole.append(
            HoleM50(
                Zh=8,
                W0=50e-3,
                W1=2e-3,
                W2=1e-3,
                W3=1e-3,
                W4=20.6e-3,
                H0=17.3e-3,
                H1=1.25e-3,
                H2=0.5e-3,
                H3=6.8e-3,
                H4=1e-3,
                magnet_0=MagnetType10(Wmag=0.01, Hmag=0.02),
            )
        )
        a = 5
        pytest.set_trace()

        result = test_obj.hole[0].build_geometry()
        assert len(result) == 6
        for surf in result:
            assert type(surf) is SurfLine

        assert result[0].label[:5] == "Hole_"
        assert result[0].label[-9:] == "_R0_T0_S0"
        assert len(result[0].line_list) == 7
```

The tests will be paused when the line will be called and the terminal will allow us to debug. With it we can print variables and objects. We can exit the debug mode
by typing __exit__ or __continue__ in the terminal : __exit__ will stop and quit the tests, __continue__ will stop the debug mode and continue the tests.
__Notice__ : It's impossible to test the lines after the line print by the debugger at the start of the debug mode.

Here is an example :
```
Tests\Methods\Slot\test_HoleM50_meth.py .......
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> PDB set_trace (IO-capturing turned off) >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
> c:\users\mesure_01\desktop\pyleecan\tests\methods\slot\test_holem50_meth.py(247)test_build_geometry_two_hole_with_magnet()
-> result = test_obj.hole[0].build_geometry()
(Pdb) print(a)
5
```







