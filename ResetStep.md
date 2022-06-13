
### Reset Test Step Plugin development
[Previous](https://github.com/csprings/Code-Repo/blob/gh-pages/index.md)

1.	Select ***Reset.py*** file and add(from) VNA class to this Reset.py file.
```python
from .VNA import *
```
2.	Change ***attribute*** of the Reset TestStep. 
```python
@Attribute(DisplayAttribute, "Reset", "Add a description here", "Add a group name here")
```
---->
```python
@Attribute(DisplayAttribute, "Reset", "Reset the instrument to default setting", "VNA") 
```
3.  Add ***setting attribute***, which will choose and connect the instrument. Similar to the VNA.py, reuse exist example code.
```python
# Add VNA instrument to the test step
Prop = self.AddProperty("vna", None, VNA)
Prop.AddAttribute(DisplayAttribute, "Instrument", "The instrument to connect", "Resources")
```
And delete or comment out for the next attribute with ***“ctrl +/”***.

4.	Scroll down to **Run()** method and add the below 2 lines to send ***“SYSTem:FPReset”*** SCPI command and left a log to notifying the instrument has been reset.
```python
self.vna._io.ScpiCommand("SYSTem:FPReset")
self.Info("Insturement has been reset")
```

5. Move to the next step [S-Parameter setup plugin](https://github.com/csprings/Code-Repo/blob/gh-pages/SParaTest.md)
