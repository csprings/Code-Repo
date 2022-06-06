## Welcome to Insight PathWave Test Automation Class

Here is the webpage, you can copy and paste some code during the training to shorten typing time.


### Implement VNA Instrument Plugin

1. Select ***VNA.py*** file. Add below command line ***before VNA class***.
```python
from OpenTap.Plugins.BasicSteps import GenericScpiInstrument
```
2. Replacing the below ***Atrribute*** line with the below new line
```python
@Attribute(DisplayAttribute, "VNA", "Add a description here", "Add a group name here")
```
---->
```python
@Attribute(DisplayAttribute, "VNA", "add VNA Network Analyzer", "VNA")
```
3. On the ***__init__*** function copy and paste below lines
```python
#create _io instance to use GenericScpiInstrument
self._io = GenericScpiInstrument()
#Name of instrument that showing Instrument bar in Test Automation software
self.Name = "VNA"

#VISA address Property
Prop = self.AddProperty("visa_address", "TCPIP0::127.0.0.1::hislip0::INSTR", String)
Prop.AddAttribute(DisplayAttribute, "VISA Address", "VISA Address of the instrument to connect", "VISA")

#VISA connection Timeout setting property
Prop = self.AddProperty("io_timeout", 5000, Int32)
Prop.AddAttribute(DisplayAttribute, "IO Timeout", "Timeout of the connection", "VISA")
Prop.AddAttribute(UnitAttribute, "sec", PreScaling = 1000)
```

4.	On the ***Open*** function, add below 3 lines of the code, that will add default visa_address to _io instance and open the instrument connection when it calls Open() method. 
```python
self._io.VisaAddress  = self.visa_address
self._io.IoTimeout = self.io_timeout
self._io.Open()
```

5.	On the ***Close*** function, add a line, that close the instrument connection 
```python
self._io.Close()
```

6. Let"s move to next step [Reset Test Step](https://csprings.github.io/Code-Repo/ResetStep.html)
