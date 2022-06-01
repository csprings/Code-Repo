## Welcome to Insight PathWave Test Automation Class

Here is the webpage, you can copy and paste some code during the training to shorten typing time.


### Implement VNA Instrument Plugin

in VNA.py file, add below codes
```python
from OpenTap.Plugins.BasicSteps import GenericScpiInstrument
```
replacing the below Atrribute line with the below new line
```C#
@Attribute(DisplayAttribute, “VNA”, “Add a description here”, “Add a group name here”)
->
@Attribute(DisplayAttribute, “VNA”, “add VNA Network Analyzer”, “VNA”)
```
add the below 2 linese of command, that will connect Keysight VISA Library to VNA class and name of the instrument
```python
Self._io = GenericScpiInstrument()
Self.Name = “VNA”
```
Set the setting that add instrument setting property to Setting window in Test Automation software.
```C#
Prop = self.AddProperty(“string_property_example”, “string example”, String)
Prop.AddAttribute(DisplayAttribute, “add a display name here”, “Add a description here”, “Add a group name here”)
->
Prop = self.AddProperty(“visa_address”, “TCPIP0::127.0.0.1::hislip0::INSTR”, String)
Prop.AddAttribute(DisplayAttribute, “VISA Address”, “VISA Address of the instrument to connect”, “VISA”)
```


