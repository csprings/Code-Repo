## Welcome to Insight PathWave Test Automation Class

Here is the webpage, you can copy and paste some code during the training to shorten typing time.


### Implement VNA Instrument Plugin

1. Select ***VNA.py*** file. Add below command line ***before VNA class***.
```python
from OpenTap.Plugins.BasicSteps import GenericScpiInstrument
```
2. Replacing the below ***Atrribute*** line with the below new line
```python
@Attribute(DisplayAttribute, “VNA”, “Add a description here”, “Add a group name here”)
```
->
```python
@Attribute(DisplayAttribute, "VNA", "add VNA Network Analyzer", "VNA")
```
3. Add the below 2 linese of command, that will connect Keysight VISA Library to VNA class and name of the instrument
```python
self._io = GenericScpiInstrument()
self.Name = "VNA"
```
4. Set the setting that add instrument setting property to Setting window in Test Automation software.
```python
Prop = self.AddProperty(“string_property_example”, “string example”, String)
Prop.AddAttribute(DisplayAttribute, “add a display name here”, “Add a description here”, “Add a group name here”)
```
->
```python
Prop = self.AddProperty("visa_address", "TCPIP0::127.0.0.1::hislip0::INSTR", String)
Prop.AddAttribute(DisplayAttribute, "VISA Address", "VISA Address of the instrument to connect", "VISA")
```
5.	Add one more property that can change the IO timeout. 
```python
Prop = self.AddProperty(“integer_property_example”, 0, Int32)
Prop.AddAttribute(DisplayAttribute, “add a display name here”, “Add a description here”, “Add a group name here”)
Prop.AddAttribute(UnitAttribute, “Add a display unit here”)
```
->
```python
Prop = self.AddProperty("io_timeout", 5000, Int32)
Prop.AddAttribute(DisplayAttribute, "IO Timeout", "Timeout of the connection", "VISA")
Prop.AddAttribute(UnitAttribute, "sec", PreScaling = 1000)
```

6.	On the Open Method, add below 2 line of the code, that will add default visa_address to _io instance and open the instrument connection when it calls Open() method. 
```python
self._io.VisaAddress  = self.visa_address
self._io.IoTimeout = self.io_timeout
self._io.Open()
```
7.	On the Close Method, add a line, that close the instrument connection 
```python
self._io.Close()
```

8. Let"s move to next step [Reset Test Step](https://csprings.github.io/Code-Repo/ResetStep.html)
