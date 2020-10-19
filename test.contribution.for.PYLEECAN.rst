##################
How to write test for Pyleecan
##################

First of all, we are currently using **pytest** to perform testing in Pyleecan. 
Through this tutorial, I’ll explain you the conventions we established to build up some test from scratch. 
If you are not used to write test in pytest, check this `Test Development Guideline <https://github.com/Eomys/pyleecan-doc/blob/master/test.contribution.rst>`__.

An example of a Pyleecan test with a fixture method
```````````````

Once you know how to write a pytest test, you'll be able to understand the following code (There is just the beginning of the test class):


.. code-block:: python


        # -*- coding: utf-8 -*-

        from os.path import join
        import pytest

        import matplotlib.pyplot as plt
        from numpy import pi

        from pyleecan.Classes.Frame import Frame
        from pyleecan.Classes.LamHole import LamHole
        from pyleecan.Classes.LamSlotWind import LamSlotWind
        from pyleecan.Classes.MachineIPMSM import MachineIPMSM
        from pyleecan.Classes.Magnet import Magnet
        from pyleecan.Classes.Shaft import Shaft
        from pyleecan.Classes.VentilationCirc import VentilationCirc
        from pyleecan.Classes.VentilationPolar import VentilationPolar
        from pyleecan.Classes.HoleM50 import HoleM50
        from pyleecan.Classes.BoreFlower import BoreFlower
        from Tests import save_plot_path as save_path


        """unittest for Machine with Hole 50 plot"""


        @pytest.mark.PLOT
        class Test_Hole_50_plot(object):
            @pytest.fixture
            def machine(self):
                """Run at the begining of every test to setup the machine"""
                plt.close("all")
                test_obj = MachineIPMSM()
                test_obj.rotor = LamHole(
                    is_internal=True, Rint=0.021, Rext=0.075, is_stator=False, L1=0.7
                )
                test_obj.rotor.bore = BoreFlower(N=8, Rarc=0.05, alpha=pi / 8)
                test_obj.rotor.hole = list()
                test_obj.rotor.hole.append(
                    HoleM50(
                        Zh=8,
                        W0=50e-3,
                        W1=2e-3,
                        W2=1e-3,
                        W3=1e-3,
                        W4=20.6e-3,
                        H0=17.3e-3,
                        H1=3e-3,
                        H2=0.5e-3,
                        H3=6.8e-3,
                        H4=0,
                    )
                )
                test_obj.rotor.axial_vent = list()
                test_obj.rotor.axial_vent.append(
                    VentilationCirc(Zh=8, Alpha0=0, D0=5e-3, H0=40e-3)
                )
                test_obj.rotor.axial_vent.append(
                    VentilationCirc(Zh=8, Alpha0=pi / 8, D0=7e-3, H0=40e-3)
                )
                test_obj.shaft = Shaft(Drsh=test_obj.rotor.Rint * 2, Lshaft=1.2)

                test_obj.stator = LamSlotWind(
                    Rint=0.078, Rext=0.104, is_internal=False, is_stator=True, L1=0.8
                )
                test_obj.stator.slot = None
                test_obj.stator.axial_vent.append(
                    VentilationPolar(Zh=8, H0=0.08, D0=0.01, W1=pi / 8, Alpha0=pi / 8)
                )
                test_obj.stator.axial_vent.append(
                    VentilationPolar(Zh=8, H0=0.092, D0=0.01, W1=pi / 8, Alpha0=0)
                )
                test_obj.frame = Frame(Rint=0.104, Rext=0.114, Lfra=1)

                return test_obj

Here is a test class for pytest. Most of the tests in Pyleecan are in a test class. 
It is very important to put a pytest mark before the declaration of the class.
Moreover, every test classes must starting their name with an uppercase. If not, pytest will not run it. 
In an another way, every test classes doesn't have to inherit of **object**.

At the line with **pytest.fixture**, we are using a fixture, it's one of the two way to make a set_up function. You'll see the second later.
So, the fixture function is just a simple function that we launch before every test functions in the test class. Here they are :

.. code-block:: python

            def test_Lam_Hole_50_W01(self, machine):
                """Test machine plot hole 50 with W1 > 0 and both magnets"""
                machine.plot()
                fig = plt.gcf()
                fig.savefig(join(save_path, "test_Lam_Hole_s50_Machine.png"))
                assert len(fig.axes[0].patches) == 87

                machine.rotor.plot()
                fig = plt.gcf()
                fig.savefig(join(save_path, "test_Lam_Hole_s50_Rotor_W01.png"))
                # 2 for lam + (3*2)*8 for holes + 16 vents
                assert len(fig.axes[0].patches) == 66

                machine.rotor.axial_vent[0].plot()
                fig = plt.gcf()
                fig.savefig(join(save_path, "test_Lam_Hole_CircVent.png"))
                assert len(fig.axes[0].patches) == 8

            def test_Lam_Hole_50_N01(self, machine):
                """Test machine plot hole 50 with W1 = 0 and both magnets"""
                machine.rotor.hole[0].W1 = 0
                machine.rotor.hole[0].magnet_0 = Magnet()
                machine.rotor.hole[0].magnet_1 = Magnet()
                machine.rotor.plot()
                fig = plt.gcf()
                fig.savefig(join(save_path, "test_Lam_Hole_s50_RotorN01.png"))
                # 2 for lam + 5*8 for holes + 16 vents
                assert len(fig.axes[0].patches) == 58

Every test functions are named like above: **test_*()**. Like said before, if you don't do that, **pytest** will not run it. 
It is very import to name your test function by describing what it does or what it concerns. Below every declarations, we are commenting the goal of the test.
To improve the understanding of your test code, you have to comment your code.

An example of a Pyleecan test without a fixture method
```````````````

.. code-block:: python

            # -*- coding: utf-8 -*-
            """
            @date Created on Wed Jan 20 14:10:24 2016
            @copyright (C) 2015-2016 EOMYS ENGINEERING.
            @author pierre_b
            """

            import sys
            from random import uniform

            from PyQt5 import QtWidgets
            from PyQt5.QtTest import QTest

            from pyleecan.Classes.LamHole import LamHole
            from pyleecan.Classes.HoleM57 import HoleM57
            from pyleecan.GUI.Dialog.DMachineSetup.SMHoleMag.PHoleM57.PHoleM57 import PHoleM57
            from Tests.GUI import gui_option  # Set unit to m

            import pytest


            @pytest.mark.GUI
            class Test_PHoleM57(object):
                """Test that the widget PHoleM57 behave like it should"""

                def setup_method(self, method):
                    """Run at the begining of every test to setup the gui"""
                    self.test_obj = LamHole(Rint=0.1, Rext=0.2)
                    self.test_obj.hole = list()
                    self.test_obj.hole.append(
                        HoleM57(H1=0.11, H2=0.12, W0=0.13, W1=0.14, W2=0.15, W3=0.17, W4=0.19)
                    )
                    self.widget = PHoleM57(self.test_obj.hole[0])

                @classmethod
                def setup_class(cls):
                    """Start the app for the test"""
                    print("\nStart Test PHoleM57")
                    cls.app = QtWidgets.QApplication(sys.argv)

                @classmethod
                def teardown_class(cls):
                    """Exit the app after the test"""
                    cls.app.quit()
                  
                  
As you can see, there is no **@pytest.fixture here**. We are using the simple setup and teardown method provided by pytest itself. Please notice that those **@classmethod** are not mandatory.
It is important to use **self.** to setup your variables in your **setup_method** to use them in the test methods.

.. code-block:: python

            def test_init(self):
                """Check that the Widget spinbox initialise to the lamination value"""

                assert self.widget.lf_H1.value() == 0.11
                assert self.widget.lf_H2.value() == 0.12
                assert self.widget.lf_W0.value() == 0.13
                assert self.widget.lf_W1.value() == 0.14
                assert self.widget.lf_W2.value() == 0.15
                assert self.widget.lf_W3.value() == 0.17
                assert self.widget.lf_W4.value() == 0.19

                self.test_obj.hole[0] = HoleM57(
                    H1=0.21, H2=0.22, W0=0.23, W1=0.24, W2=0.25, W3=0.27, W4=0.29
                )
                self.widget = PHoleM57(self.test_obj.hole[0])
                assert self.widget.lf_H1.value() == 0.21
                assert self.widget.lf_H2.value() == 0.22
                assert self.widget.lf_W0.value() == 0.23
                assert self.widget.lf_W1.value() == 0.24
                assert self.widget.lf_W2.value() == 0.25
                assert self.widget.lf_W3.value() == 0.27
                assert self.widget.lf_W4.value() == 0.29

            def test_set_W0(self):
                """Check that the Widget allow to update W0"""
                # Clear the field before writing the new value
                self.widget.lf_W0.clear()
                QTest.keyClicks(self.widget.lf_W0, "0.31")
                self.widget.lf_W0.editingFinished.emit()  # To trigger the slot

                assert self.widget.hole.W0 == 0.31
                assert self.test_obj.hole[0].W0 == 0.31

            def test_set_W1(self):
                """Check that the Widget allow to update W1"""
                self.widget.lf_W1.clear()
                QTest.keyClicks(self.widget.lf_W1, "0.32")
                self.widget.lf_W1.editingFinished.emit()  # To trigger the slot

                assert self.widget.hole.W1 == 0.32
                assert self.test_obj.hole[0].W1 == 0.32
                
Here, each computer variable are accessed with the **self.** syntax. It's because we are using a setup function and not a fixture function.
In the same way of the first method, you have to comment your code.


Multiple mark in a test file
```````````````

You may ask: "How can we put multiple mark to specific tests in a test class ?". You can't. If you put marks for a class, they will be priority on those which are for a simple test. If you want to keep a class, you can just put marks on every test functions. **Caution :** Doing that can slow the test execution.


Where to create a test file
```````````````

In the **Tests** directory, you can find some various subdirectories. 
If you're writing a test which it concerns the GUI, you'll have to put your test in the subdirectory **GUI**.
In a same way, if you're writing a test which it concerns a Plot, you'll have to put your test in the subdirectory **Plot**.


Black: The code formatter
```````````````

::

                 pip install black
                 python -m black {source_file_or_directory}
        
You can install the package Black by using the line code above. Black allow you to enhance your code by formatting it.
Feel free to check the documentation : `Black <https://black.readthedocs.io/en/stable/>`__.
