### VNA Test Plugin development

1.	We will use several numbers for this test step and one check box that will use Boolean so change the below line
```python
from System import String, Int32 
-> 
from System Import String, Int32, Double, Boolean
```

2. Need to connect VNA class to this file, so add the below line.
```python
from .VNA import *
```
3. One of the settings is choosing a Measurement item from dropdown box. For that feature, we need to make an Enumeration function for dropdown box item.
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

4.	Let’s change the Test Step Display attribute so that we can see both IDN and SParamterTest step under the same group. 
```python
@Attribute(DisplayAttribute, "SParameterTest", "Add a description here", "Add a group name here")
->
@Attribute(DisplayAttribute, "S-ParameterTest", "Sparameter Test", "VNA")
```

5.	Let’s add Settings, first is adding instrument setting. Just change the lines under #String Property example
```python
prop = self.AddProperty("string_property_example", "string example", String)
prop.AddAttribute(DisplayAttribute, "Add a display name here", "Add a description here", "Add a group name here")
->
prop = self.AddProperty("vna", None, VNA)
prop.AddAttribute(DisplayAttribute, "Instrument", "The instrument to connect", "Resources", 1)
```

6.	Copy previous 2 lines of coding for instrument and change like the below, this is for Channel number
```python
prop = self.AddProperty("VnaChannel", 1, Int32)
prop.AddAttribute(DisplayAttribute, "Channel", "Channel", "VNA Setup", 2)
```

7.	Do the same thing, copy previous 2 lines and change like below 
```python
prop = self.AddProperty("VnaWindow", 1, Int32)
prop.AddAttribute(DisplayAttribute, "Window", "Window", "VNA Setup", 3)
```

8.	This case, we will reuse the codes below ***#Integer property*** example, change the line like the below.
```python
prop = self.AddProperty("integer_property_example", 0, Int32)
prop.AddAttribute(DisplayAttribute, "Add a display name here", "Add a description here", "Add a group name here")
prop.AddAttribute(UnitAttribute, "Add a display unit here")
->
prop = self.AddProperty("StartFrequency", 1E+09, Double)
prop.AddAttribute(DisplayAttribute, "Start Frequency", "Start Frequency of the sweep", "VNA Setup", 4)
prop.AddAttribute(UnitAttribute, "Hz")
```

9.	Now copy previous settings, 3 lines of coding, and change like the below.
```python
prop = self.AddProperty("StopFrequency", 2E+09, Double)
prop.AddAttribute(DisplayAttribute, "Stop Frequency", "Stop Frequency of the sweep", "VNA Setup", 5)
prop.AddAttribute(UnitAttribute, "Hz")
```

10.	Same thing, copy and paste previous lines, but we don’t need “UnitAttribute”, so delete the last line. This is the Autoscale check box setting.
```python
prop = self.AddProperty("Autoscale", True, Boolean)
prop.AddAttribute(DisplayAttribute, "AutoScale", "Autoscale", "VNA Setup", 6)
```

11. Lastly, copy and paste the previous setting and change it like below. This is manual input for the measurement parameter. 
```python
prop = self.AddProperty("Measurement", "S21", String)
prop.AddAttribute(DisplayAttribute, "Measurement", "put S21, S11, S22, S12", "Measurement", 7)
```
