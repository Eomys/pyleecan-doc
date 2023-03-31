# How to make tests for the GUI (Graphical User Interface)

Pyleecan also includes some tests for the GUI. These tests enable to simulate the interaction of a user with the GUI (a button is clicked, a field is edited...) and check that the GUI reacted as expected. Here is a first example of such tests:

## PHoleM50

[This class](https://github.com/EOMYS-Public/pyleecan/blob/master/pyleecan/GUI/Dialog/DMachineSetup/SMSlot/PMSlot10/PMSlot10.py) is the perfect example of a GUI class to test.
To test the widget of the class, it is advised to check which are the events that the user can perform on it. In the init of the class, there are those lines:

```py
        # Connect the signal
        self.lf_W0.editingFinished.connect(self.set_W0)
        self.lf_Wmag.editingFinished.connect(self.set_Wmag)
        self.lf_H0.editingFinished.connect(self.set_H0)
        self.lf_Hmag.editingFinished.connect(self.set_Hmag)
```

PMSlot10 contains different possible interactions: __lf_W0__, __lf_Wmag__ ... __lf_Hmag__. Those variables are part of the widget that can be updated by the user. The test will modify them to simulate a user interaction. The __lf_W0__ is a widget to enter a float (known because it starts with a "lf"). To test the function self.set_W0 which is connected to the __lf_W0__ by a signal (__editingFinished__), we need to clear the field, enter a value in it as a user would do
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
And after that we are emitting the signal. Once the signal is emitted, the function __set_W0__ of PMSlot10 is called:

```py
    def set_W0(self):
        """Signal to update the value of W0 according to the line edit

        Parameters
        ----------
        self : PMSlot10
            A PMSlot10 object
        """
        self.slot.W0 = self.lf_W0.value()
        self.w_out.comp_output()
        # Notify the machine GUI that the machine has changed
        self.saveNeeded.emit()
```

If the function is correctly working, the widget will update the value W0 of the concerned slot (here SlotM10):

```py
        assert widget.hole.W0 == 0.31
```

Finally to make this test work, the widget needs to be initialized. Here is a complete example: 

```py
class TestPMSlot10(object):
    """Test that the widget PMSlot10 behave like it should"""

    def setup_method(self):
        """Function that will be run before every tests"""
        self.test_obj = LamSlotMag(Rint=0.1, Rext=0.2)
        self.test_obj.slot = SlotM10(H0=0.10, W0=0.13, Wmag=0.14, Hmag=0.15)
        self.widget = PMSlot10(self.test_obj)

    @classmethod
    def setup_class(cls):
        """Start the app for the test (only done one time for the class test)"""
        print("\nStart Test TestPMSlot10")
        if not QtWidgets.QApplication.instance():
            cls.app = QtWidgets.QApplication(sys.argv)
        else:
            cls.app = QtWidgets.QApplication.instance()

    @classmethod
    def teardown_class(cls):
        """Exit the app after the test (only done one time for the class test)"""
        cls.app.quit()

    def test_init(self):
        """Check that the Widget spinbox initialise to the lamination value"""

        assert self.widget.lf_H0.value() == 0.10
        assert self.widget.lf_Hmag.value() == 0.15
        assert self.widget.lf_W0.value() == 0.13
        assert self.widget.lf_Wmag.value() == 0.14

    def test_set_W0(self):
        """Check that the Widget allow to update W0"""
        # Check Unit
        assert self.widget.unit_W0.text() == "[m]"
        # Change value in GUI
        self.widget.lf_W0.clear()
        QTest.keyClicks(self.widget.lf_W0, "0.31")
        self.widget.lf_W0.editingFinished.emit()  # To trigger the slot

        assert self.widget.slot.W0 == 0.31
        assert self.test_obj.slot.W0 == 0.31
```

We are using a setup method to setup the widget. [Here](make.a.typical.pyleecan.test.md)
is a tutorial to explain what it is.

We saw how to test the user input in a field. We can also test comboboxes, which is actually easier. Those lines are connecting some widgets containing comboboxes to the need to save when they are edited :

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
        widget.w_mat_0.c_mat_type.setCurrentIndex(0)

        assert self.widget.w_mat_0.c_mat_type.currentText() == "Magnet1"
        assert self.test_obj.hole[0].mat_void.name == "Magnet1"

        self.widget.w_mat_0.c_mat_type.setCurrentIndex(1)

        assert self.widget.w_mat_0.c_mat_type.currentText() == "Magnet2"
        assert self.test_obj.hole[0].mat_void.name == "Magnet2"
```

Similarly to the previous example, __self.test_obj__ is set in the setup_method. When we change the current index, a save signal is sent and the type will be changed automatically.

## To go further

Pytest allow to debug the code during the test. [Here is an another tutorial.](how.to.debug.with.pytest.md)
