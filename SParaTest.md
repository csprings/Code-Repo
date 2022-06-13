### S-Parameter Test Setup Plugin development
[Previous](https://github.com/csprings/Code-Repo/blob/gh-pages/ResetStep.md)

1.	We will use several numbers for this test step and one ***check box*** that will use ***Boolean*** so change the below line
```python
from System import String, Int32 
```
----> 
```python
from System Import String, Int32, Double, Boolean
```

2. Need to connect ***VNA class*** to this file, so add the below line.
```python
from .VNA import *
```
3. One of the ***settings*** is choosing a Measurement item from dropdown box. For that feature, we need to make an Enumeration function for dropdown box item.
```python
from enum import Enum
```
```python
class Measurement(Enum):
	S11 = ("S11", "Return loss of port 1")
	S21 = ("S21", "Insertion loss from port 1 to port 2")
	S22 = ("S22", "Return loss of port 2")
	S12 = ("S12", "Insertion loss from port 2 to port 1")
```

4. Let’s change the Test Step Display attribute so that we can see both IDN and SParamterTest step under the same group. 
```python
@Attribute(DisplayAttribute, "SParameterTest", "Add a description here", "Add a group name here")
```
---->
```python
@Attribute(DisplayAttribute, "S-ParameterTest", "Sparameter Test", "VNA")
```

5. Let’s add ***Settings***, copy below lines of the code, and paste it to the ***init*** function
```python
# Add Instrument instance to this test step
prop = self.AddProperty("vna", None, VNA)
prop.AddAttribute(DisplayAttribute, "Instrument", "The instrument to connect", "Resources", 1)

# add Channel information to settting, this use Int or Double
prop = self.AddProperty("VnaChannel", 1, Int32)
prop.AddAttribute(DisplayAttribute, "Channel", "Channel", "VNA Setup", 2)

# add Window information to settting, this use Int or Double
prop = self.AddProperty("VnaWindow", 1, Int32)
prop.AddAttribute(DisplayAttribute, "Window", "Window", "VNA Setup", 3)

# start frequency setting, use Double, it will provide more precision setting
prop = self.AddProperty("StartFrequency", 1E+09, Double)
prop.AddAttribute(DisplayAttribute, "Start Frequency", "Start Frequency of the sweep", "VNA Setup", 4)
prop.AddAttribute(UnitAttribute, "Hz")

# stop frequency setting, use Double, it will provide more precision setting
prop = self.AddProperty("StopFrequency", 2E+09, Double)
prop.AddAttribute(DisplayAttribute, "Stop Frequency", "Stop Frequency of the sweep", "VNA Setup", 5)
prop.AddAttribute(UnitAttribute, "Hz")

# check box setting for Autoscale, default is checked(True) use boolean(True/False)
prop = self.AddProperty("Autoscale", True, Boolean)
prop.AddAttribute(DisplayAttribute, "AutoScale", "Autoscale", "VNA Setup", 6)

# setup Measurement parameter from Enum, default is S21
prop = self.AddProperty("Measure", Measurement.S21, Measurement)
prop.AddAttribute(DisplayAttribute, "Measurement", "put S21, S11, S22, S12", "Measurement", 7)

# the number, we will use for indentifying name of measurement
prop = self.AddProperty("MeasurementNum", 1, Int32)
prop.AddAttribute(DisplayAttribute, "Measurement Number", "Number for the measurement", "Measurement", 8)
```

6. Scroll down to ***Run()*** function, Add ***SCPI commands***. 
```python
# To enable new window in the software we will replace the window number to the VnaWindow
self.vna._io.ScpiCommand(f"DISPlay:WINDow{self.VnaWindow}:STATE ON")

# Setup the measurement for the channel
self.vna._io.ScpiCommand(f"CALCulate{self.VnaChannel}:PARameter:DEFine:EXT 'Meas{self.MeasurementNum}', '{self.Measure.value[0]}'")

# This command will add a trace to the newly created window
self.vna._io.ScpiCommand(f"DISPlay:WINDow{self.VnaWindow}:TRACe{self.MeasurementNum}:FEED 'Meas{self.MeasurementNum}'")

# It will setup the frequency span from 1GHz, default Start frequency to 2GHz, default Stop frequency.
self.vna._io.ScpiCommand(f"SENSe{self.VnaChannel}:FREQuency:STARt {self.StartFrequency}")
self.vna._io.ScpiCommand(f"SENSe{self.VnaChannel}:FREQuency:STOP {self.StopFrequency}")
```

7. Now, finished setting for the trace, left setting, we’ve not used is the AutoScale check box. If the box is checked, we need to perform autoscale of the trace. So need to check whether the box is checked(True) or unchecked(False). Will add “If” function to check this. In Python, indentation is most important, so don’t forget to check, there has a different indentation between “if” and “self.vna” line
```python
if self.Autoscale:
	self.vna._io.ScpiCommand(f"DISP:WIND{self.VnaWindow}:Y:AUTO")
```
