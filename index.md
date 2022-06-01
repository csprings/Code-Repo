## Welcome to Insight PathWave Test Automation Class

Here is the webpage, you can copy and paste some code during the training to shorten typing time.
[Reset Test Step](https://csprings.github.io/Code-Repo/ResetStep.html)

### Implement VNA Instrument Plugin

1. in VNA.py file, add below codes
```markdown
from OpenTap.Plugins.BasicSteps import GenericScpiInstrument
```
2. replacing the below Atrribute line with the below new line
```markdown
@Attribute(DisplayAttribute, “VNA”, “Add a description here”, “Add a group name here”)
->
@Attribute(DisplayAttribute, “VNA”, “add VNA Network Analyzer”, “VNA”)
```
3. add the below 2 linese of command, that will connect Keysight VISA Library to VNA class and name of the instrument
```markdown
Self._io = GenericScpiInstrument()
Self.Name = “VNA”
```
4. Set the setting that add instrument setting property to Setting window in Test Automation software.
```markdown
Prop = self.AddProperty(“string_property_example”, “string example”, String)
Prop.AddAttribute(DisplayAttribute, “add a display name here”, “Add a description here”, “Add a group name here”)
->
Prop = self.AddProperty(“visa_address”, “TCPIP0::127.0.0.1::hislip0::INSTR”, String)
Prop.AddAttribute(DisplayAttribute, “VISA Address”, “VISA Address of the instrument to connect”, “VISA”)
```
5.	On the Open Method, add below 2 line of the code, that will add default visa_address to _io instance and open the instrument connection when it calls Open() method. 
```
self._io.VisaAddress  = self.visa_address
self._io.Open()
```
6.	On the Close Method, add a line, that close the instrument connection 
```
self._io.Close()
```


