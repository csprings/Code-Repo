## Welcome to Insight PathWave Test Automation Class

Here is the webpage, you can copy and paste some code during the training to shorten typing time.


### Reset Test Step Plugiu development

1.	Select Reset.py file and add(from) VNA class to this IDN.py file.
```
from VNA import *
```
2.	Change attribute of the IDN TestStep. 
```
@Attribute(DisplayAttribute, “Reset”, “Add a description here”, “Add a group name here”)
->
@Attribute(DisplayAttribute, “Reset”, “Reset the instrument to default setting”, “VNA”) 
```
3.  Add setting attribute, which will choose and connect the instrument. Similar to the VNA.py, reuse exist example code.
```
Prop = self.AddProperty(“string_property_example”, “string example”, String)
Prop.AddAttribute(DisplayAttribute, “add a display name here”, “Add a description here”, “Add a group name here”)
-> 
Prop = self.AddProperty(“vna”, None, VNA)
Prop.AddAttribute(DisplayAttribute, “Instrument”, “The instrument to connect”, “Resources”)
And delete or comment out for the next attribute with “ctrl +/”.
```
4.	Scroll down to Run() method and add the below 2 lines to send “SYSTem:FPReset” SCPI command and left a log to notifying the instrument has been reset.
```
self.vna._io.ScpiCommand(“SYSTem:FPReset”)
self.Info(“Insturement has been reset”)
```
