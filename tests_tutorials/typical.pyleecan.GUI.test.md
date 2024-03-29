# How to make a typical pyleecan GUI test

In Pyleecan we are using test classes to test our code. A class test allow us to run it with pytest and with normal python too to enter in the debugging mode while testing. A test class also give the option to write a setup_method function that will be run before every tests (to reset the setup if modified in a test) :

```py
class Test_Hole_57_plot(object):

    """pytest for Machine with Hole 57 plot"""

    def setup_method(self, method):
        """Run at the begining of every test to setup the machine"""
        plt.close("all")
        test_obj = MachineIPMSM()
        test_obj.rotor = LamHole(
            is_internal=True, Rint=0.021, Rext=0.075, is_stator=False, L1=0.7
        )
        test_obj.rotor.hole = list()
        test_obj.rotor.hole.append(
            HoleM57(
                Zh=8,
                W0=pi * 0.8,
                W1=10e-3,
                W2=0e-3,
                W3=5e-3,
                W4=10e-3,
                H1=3e-3,
                H2=5e-3,
            )
        )
        test_obj.shaft = Shaft(Drsh=test_obj.rotor.Rint * 2, Lshaft=1.2)

        test_obj.stator = LamSlotWind(
            Rint=0.078, Rext=0.104, is_internal=False, is_stator=True, L1=0.8
        )
        test_obj.stator.slot = None
        test_obj.stator.winding = None
        test_obj.frame = Frame(Rint=0.104, Rext=0.114, Lfra=1)
        self.test_obj = test_obj
        
     def test_Lam_Hole_57_N01(self):
        """Test machine plot hole 57 with W1 = 0 and both magnets"""
        self.test_obj.rotor.hole[0].W1 = 0
        self.test_obj.rotor.hole[0].magnet_0 = Magnet()
        self.test_obj.rotor.hole[0].magnet_1 = Magnet()
        self.test_obj.rotor.plot(is_show_fig=False)
        fig = plt.gcf()
        fig.savefig(join(save_path, "test_Lam_Hole_57_s57_RotorN01.png"))
        # 2 for lam + (3*2)*8 for holes + 16 vents
        assert len(fig.axes[0].patches) == 42

    def test_Lam_Hole_57_W01(self):
        """Test machine plot hole 57 with W1 > 0 and both magnets"""
        self.test_obj.plot(is_show_fig=False)
        fig = plt.gcf()
        fig.savefig(join(save_path, "test_Lam_Hole_57_s57_Machine.png"))
        assert len(fig.axes[0].patches) == 55

        self.test_obj.rotor.plot(is_show_fig=False)
        fig = plt.gcf()
        fig.savefig(join(save_path, "test_Lam_Hole_57_s57_Rotor_W01.png"))
        # 2 for lam + (3*2)*8 for holes + 16 vents
        assert len(fig.axes[0].patches) == 50

```

In the setup_method, we set the object that will be tested. We are currently doing two tests on this class. The setup_method is mandatory to ensure that the second test will have a object not modified because of the first test. So each test will have the same setup.

For GUI test it is possible to write others setup/teardown methods :

There are two types of setup/teardown methods : for classes and for tests. In Pyleecan we use these two methods setup/teardown for classes :
```py
class TestSomething(object):
    @classmethod
    def setup_class(cls):
        """Start the app for the test"""
        print("\nStart Test TestPMSlot10")
        if not QtWidgets.QApplication.instance():
            cls.app = QtWidgets.QApplication(sys.argv)
        else:
            cls.app = QtWidgets.QApplication.instance()

    @classmethod
    def teardown_class(cls):
        """Exit the app after the test"""
        cls.app.quit()
```
The annotation __@classmethod__ means that when the test class will be instancied, it will run the __setup_class__ at the start and when the class will be destroyed, __teardown_class__ will be called. In Pyleecan we use them to run the application of the GUI and to close it.

For setup/teardown methods for tests, here is two examples :
```py
class TestSomething(object):
    def setup_method(self):
        print("setup_method")

    def teardown_method(self):
        print("teardown_method")
```
For each tests in the class, the __setup_method__ will be called at the start of the test and the __teardown_method__ will be called at the end of the test. It can allow us to setup some datas at the start and to remove them at the end.

## To go Further

Want to contribute? [Check those guidelines.](how.to.contribute.md)
