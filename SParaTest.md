### VNA Test Plugin development

1.	We will use several numbers for this test step and one check box that will use Boolean so change the below line
```
from System import String, Int32 
-> 
from System Import String, Int32, Double, Boolean
```

2. Need to connect VNA class to this file, so add the below line.
```
from .VNA import *
```

3.	Let’s change the Test Step Display attribute so that we can see both IDN and SParamterTest step under the same group. 
```
@Attribute(DisplayAttribute, "SParameterTest", "Add a description here", "Add a group name here")
->
@Attribute(DisplayAttribute, "S-ParameterTest", "Sparameter Test", "VNA")
```

4.	Let’s add Settings, first is adding instrument setting. Just change the lines under #String Property example
```
prop = self.AddProperty("string_property_example", "string example", String)
prop.AddAttribute(DisplayAttribute, "Add a display name here", "Add a description here", "Add a group name here")
->
prop = self.AddProperty("vna", None, VNA)
prop.AddAttribute(DisplayAttribute, "Instrument", "The instrument to connect", "Resources", 1)
```

5.	Copy previous 2 lines of coding for instrument and change like the below, this is for Channel number
```
prop = self.AddProperty("VnaChannel", 1, Int32)
prop.AddAttribute(DisplayAttribute, "Channel", "Channel", "VNA Setup", 2)
```

6.	Do the same thing, copy previous 2 lines and change like below 
```
prop = self.AddProperty("VnaWindow", 1, Int32)
prop.AddAttribute(DisplayAttribute, "Window", "Window", "VNA Setup", 3)
```

7.	This case, we will reuse the codes below ***#Integer property*** example, change the line like the below.
```
prop = self.AddProperty("integer_property_example", 0, Int32)
prop.AddAttribute(DisplayAttribute, "Add a display name here", "Add a description here", "Add a group name here")
prop.AddAttribute(UnitAttribute, "Add a display unit here")
->
prop = self.AddProperty("StartFrequency", 1E+09, Double)
prop.AddAttribute(DisplayAttribute, "Start Frequency", "Start Frequency of the sweep", "VNA Setup", 4)
prop.AddAttribute(UnitAttribute, "Hz")
```

8.	Now copy previous settings, 3 lines of coding, and change like the below.
```
prop = self.AddProperty("StopFrequency", 2E+09, Double)
prop.AddAttribute(DisplayAttribute, "Stop Frequency", "Stop Frequency of the sweep", "VNA Setup", 5)
prop.AddAttribute(UnitAttribute, "Hz")
```

9.	Same thing, copy and paste previous lines, but we don’t need “UnitAttribute”, so delete the last line. This is the Autoscale check box setting.
```
prop = self.AddProperty("Autoscale", True, Boolean)
prop.AddAttribute(DisplayAttribute, "AutoScale", "Autoscale", "VNA Setup", 6)
```

10. Lastly, copy and paste the previous setting and change it like below. This is manual input for the measurement parameter. 
```
prop = self.AddProperty("Measurement", "S21", String)
prop.AddAttribute(DisplayAttribute, "Measurement", "put S21, S11, S22, S12", "Measurement", 7)
```
