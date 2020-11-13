# How to make tests for the GUI (Graphical User Interface)

Pyleecan also includes some tests for the GUI. These tests enable to simulate the interaction of a user with the GUI (a button is clicked, a field is edited...) and check that the GUI reacted as expected. Here is a first example of such tests:

## PHoleM50

[This class](https://github.com/Eomys/pyleecan/blob/master/pyleecan/GUI/Dialog/DMachineSetup/SMHoleMag/PHoleM50/PHoleM50.py) is the perfect example of a GUI class to test.
To test the widget of the class, it is advised to check which are the events that the user can perform on it. In the init of the class, there are those lines:

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

PHoleM50 contains different possible interactions: __lf_W0__, __lf_W1__ ... __w_mat_2__. Those variables are part of the widget that can be updated by the user. The test will modify them to simulate a user interaction. The __lf_W0__ is a widget to enter a float (known because it starts with a "lf"). To test the function self.set_W0 which is connected to the __lf_W0__ by a signal (__editingFinished__), we need to clear the field, enter a value in it as a user would do
and manually call the signal __editingFinished__:

```py
    def test_set_W0(self, setup):
        """Check that the Widget allow to update W0"""
        # Clear the field before writing the new value
        widget.lf_W0.clear()
        QTest.keyClicks(widget.lf_W0, "0.31")
        widget.lf_W0.editingFinished.emit()  # To trigger the slot

        assert widget.hole.W0 == 0.31
```

A specific method of QTest (to import: __from PySide2.QtTest import QTest__) is used to fill the field with the value __0.31__ as if it were the user.
And after that we are emitting the signal. Once the signal is emitted, the function __set_W0__ of PHoleM50 is called:

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

If the function is correctly working, the widget will update the value W0 of the concerned hole (here HoleM50):

```py
        assert widget.hole.W0 == 0.31
```

Finally to make this test work, the widget needs to be initialized. Here is a complete example: 

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

We are using a fixture to setup the widget. [Here](make.setup.function.md)
is a tutorial to explain what it is.

We saw how to test the user input in a field. We can also test comboboxes, which is actually easier. In PHoleM50 we have these lines that we haven't spoken of yet:

```py
        self.w_mat_0.saveNeeded.connect(self.emit_save)
        self.w_mat_1.saveNeeded.connect(self.emit_save)
        self.w_mat_2.saveNeeded.connect(self.emit_save)
```

__W_mat_0__, __w_mat_1__ and __w_mat_2__ are the widgets that are defining the materials of the magnet of the Lamination. To trigger those __saveNeeded__ signals,
we can simply trigger the index of the combobox c_mat_type: 

```py
    def test_set_material_0(self, setup):
        """Check that you can change the material of magnet_0"""
        setup["widget"].w_mat_0.c_mat_type.setCurrentIndex(0)

        assert setup["widget"].w_mat_0.c_mat_type.currentText() == "Magnet1"
        assert setup["test_obj"].hole[0].mat_void.name == "Magnet1"

        setup["widget"].w_mat_0.c_mat_type.setCurrentIndex(1)

        assert setup["widget"].w_mat_0.c_mat_type.currentText() == "Magnet2"
        assert setup["test_obj"].hole[0].mat_void.name == "Magnet2"
```

Similarly to the previous example, __setup["test_obj"]__ is set in a setup function. When we change the current index, a save signal is sent and the type will be changed automatically.

## To go further

Pytest allow to debug the code during the test. [Here is an another tutorial.](how.to.debug.with.pytest.md)
