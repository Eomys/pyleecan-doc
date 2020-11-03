# How to make test for the GUI (Graphical User Interface)

Something that can be hard to guess is how to unit test the GUI. How is it possible to interact with the GUI during tests without an user can interact with. Because a test is
automatic, no one has to interact with them when they are launched. In fact, we can test the GUI by simulating an interaction with the GUI. But first, let's give a look this class :

## PHoleM50

This class is [the perfect example](https://github.com/Eomys/pyleecan/blob/master/pyleecan/GUI/Dialog/DMachineSetup/SMHoleMag/PHoleM50/PHoleM50.py) of a GUI class to test.
To test the widget of the class, it is advised to check which are the events that the user can perform on it. In the init of the class, there is those line :

```py
        # Connect the signal
        self.lf_W0.editingFinished.connect(self.set_W0)
        self.lf_W1.editingFinished.connect(self.set_W1)
        self.lf_W2.editingFinished.connect(self.set_W2)
        self.lf_W3.editingFinished.connect(self.set_W3)
        self.lf_W4.editingFinished.connect(self.set_W4)
        self.lf_H0.editingFinished.connect(self.set_H0)
        self.lf_H1.editingFinished.connect(self.set_H1)
        self.lf_H2.editingFinished.connect(self.set_H2)
        self.lf_H3.editingFinished.connect(self.set_H3)
        self.lf_H4.editingFinished.connect(self.set_H4)
        self.w_mat_0.saveNeeded.connect(self.emit_save)
        self.w_mat_1.saveNeeded.connect(self.emit_save)
        self.w_mat_2.saveNeeded.connect(self.emit_save)
```

PHoleM50 contains different interactions possible : __lf_W0__, __lf_W1__ ... __w_mat_2__. Those variables are parts of the widget that can be updated by the user. The test will obviously
modify them to simulate an user interaction. The __ls_W0__ is a float_field (Known because it start with a "lf"), and this field can be filled by a value by the user and also by the test.
If we want to test the function self.set_W0 which is connected to the field by a signal (__editingFinished__), we'll just have to clear the field, enter a value in it like a user'll do
and call the signal __editingFinished__. Like this :

```py
    def test_set_W0(self, setup):
        """Check that the Widget allow to update W0"""
        # Clear the field before writing the new value
        widget.lf_W0.clear()
        QTest.keyClicks(widget.lf_W0, "0.31")
        widget.lf_W0.editingFinished.emit()  # To trigger the slot

        assert widget.hole.W0 == 0.31
```

This code is not complete but it explain how the test will work with the GUI. First we clear the field to enter a value without having trouble with some text that was here at start.
Once it's done, we call a specific method of QTest (to import: __from PySide2.QtTest import QTest__) that allow us to fill the field with the value __0.31__ like if it was the user.
And after that we are emitting the signal. When th signal is emitted, the function __set_W0__ of PHoleM50 is called :

```py
    def set_W0(self):
        """Signal to update the value of W0 according to the line edit
        Parameters
        ----------
        self : PHoleM50
            A PHoleM50 widget
        """
        self.hole.W0 = self.lf_W0.value()
        self.comp_output()
        # Notify the machine GUI that the machine has changed
        self.saveNeeded.emit()
```

If the function is correclty working, the widget will update the value W0 of his hole it concerns (here HoleM50). That's why we are testing it :

```py
        assert widget.hole.W0 == 0.31
```

A question is still unanswered. How is defined __widget__ ? Here is the correct code to use : 

```py
@pytest.mark.GUI
class TestPHoleM50(object):
    """Test that the widget PHoleM50 behave like it should"""

    @pytest.fixture
    def setup(self):
        """Run at the begining of every test to setup the gui"""

        if not QtWidgets.QApplication.instance():
            self.app = QtWidgets.QApplication(sys.argv)
        else:
            self.app = QtWidgets.QApplication.instance()

        test_obj = LamHole(Rint=0.1, Rext=0.2)
        test_obj.hole = list()
        test_obj.hole.append(
            HoleM50(
                H0=0.10,
                H1=0.11,
                H2=0.12,
                W0=0.13,
                W1=0.14,
                W2=0.15,
                H3=0.16,
                W3=0.17,
                H4=0.18,
                W4=0.19,
            )
        )
        test_obj.hole[0].magnet_0.mat_type.name = "Magnet3"
        test_obj.hole[0].magnet_1.mat_type.name = "Magnet2"

        matlib = MatLib()
        matlib.dict_mat["RefMatLib"] = [
            Material(name="Magnet1"),
            Material(name="Magnet2"),
            Material(name="Magnet3"),
        ]

        widget = PHoleM50(test_obj.hole[0], matlib)

        yield {"widget": widget, "test_obj": test_obj, "matlib": matlib}

        self.app.quit()
     
    def test_set_W0(self, setup):
        """Check that the Widget allow to update W0"""
        # Clear the field before writing the new value
        setup["widget"].lf_W0.clear()
        QTest.keyClicks(setup["widget"].lf_W0, "0.31")
        setup["widget"].lf_W0.editingFinished.emit()  # To trigger the slot

        assert setup["widget"].hole.W0 == 0.31
        assert setup["test_obj"].hole[0].W0 == 0.31
```

We are using a fixture to setup the widget. [Here](https://github.com/BenjaminGabet/pyleecan-doc/blob/patch-1/Tests_Turorials/make.setup.function.md)
is a tutorial to explain what it is.

We saw how to test the user input in a field. But we can also test combobox and his choice. It is more easily. In PHoleM50 we have these lines we didn't speak yet :

```py
        self.w_mat_0.saveNeeded.connect(self.emit_save)
        self.w_mat_1.saveNeeded.connect(self.emit_save)
        self.w_mat_2.saveNeeded.connect(self.emit_save)
```

