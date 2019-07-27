
#              Arduino_Master_Delta (AMD) Tutorial    
![intropic](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/IntroPic.png?raw=true)
![Mr_Handsome](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Mr_Handsome_3.jpg?raw=true)
### _Sayad Pervez - Developer of AMD Module - [pervez2504@gmail.com](https://accounts.google.com/ServiceLogin/signinchooser?service=mail&passive=true&rm=false&continue=https%3A%2F%2Fmail.google.com%2Fmail%2F&ss=1&scc=1&ltmpl=default&ltmplcache=2&emr=1&osid=1&flowName=GlifWebSignIn&flowEntry=ServiceLogin)_
___
## Note:
### For easier importing and accessing of this module, Arduino_Master_Delta has been renamed as AMD. A new pip module named AMD has been created and all of the future updates will be implemented on AMD alone and not in Arduino_Master_Delta. Everytime you see Arduino_Master_Delta in this tutorial, replace it with AMD in your code !!
___
### Documentation at this link -> [Docs](https://pypi.org/project/Arduino-Master-Delta/)
___
## Introduction:
_**In this tutorial, we'll learn how to use the AMD python module to extract and visualize data from an Arduino. I'll assume you have basic programming experience in python and Arduino.**_

### Here are some of the projects you can build using AMD


![eg1](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Eg/4.JPG?raw=true)
![eg2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Eg/5.JPG?raw=true)
![eg3](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Eg/3.JPG?raw=true)


### Note:
>This module would work only if 'import matplotlib' statement works without throwing any error in your python console. The automatic installation feature in this module runs the same statement to check if matplotlib is preinstalled in your device. Uninstalling this module and reinstalling it could possibly throw errors like 'DLL Load Failed'.
Automatic installation feature has already been tested so don't waste your time testing this feature by uninstalling any required module. (i.e. matplotlib or pyserial) 

___
___
## ardata:
### Manual:
![manual](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/ardata/Manual.JPG?raw=true)
### Arduino Code:
```C
const int v = A0;
float Voltage;
String message;

void setup()
{
  Serial.begin(9600);
  pinMode(v,INPUT);
}

void loop()
{
  if(Serial.available()>0) // if anything is available in the serial monitor,
  message = Serial.readString();
  if(message=="Check")
  {
    Voltage = analogRead(v) * 5.0/1023.0 ; // 5/1023 is a conversion unit
    Serial.println(Voltage); // write voltage to serial monitor
  }
}
```
### Python Code:
```python
# import the module
from Arduino_Master_Delta import *

rawData = ardata(7,dynamic=True,msg='Check',lines=50,squeeze=False)
# rawData consists a list of voltages
print(rawData)

Graph(y=rawData)# x axis  is automatically generated
```
### Output:
![op](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/ardata/OP.JPG?raw=true)
### Graph:
![graph](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/ardata/G.jpeg?raw=true)
___
___
## readSerial:
### Arduino Code:
```C
int i = 0;

void setup()
{
  Serial.begin(9600);
}

void loop()
{
  Serial.println(i);
  delay(500);
  i++;
}
```
### Python Code:
```python
# importing the module,
data = readSerial(7) # returns only one value

print(data)

data2 = ardata(7,lines=20) # returns 'lines' number of values read from serial port

print(data2)

# readSerial and ardata without dynamic is not encouraged
# Since there is a possibility of getting initial data instead of
# instantaneous data
```
### Output:
![op](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/readSerial/OP.JPG?raw=true)
___
___
## writeSerial:
### Arduino Code:
```C
const int led = 7;
String message;

void setup()
{
  Serial.begin(9600);
  pinMode(led,OUTPUT);
}

void loop()
{
  if (Serial.available()>0) // if anything is available in the serial monitor,
  {
    message = Serial.readString(); // read the message as a string
    if (message=="ON")
      digitalWrite(led,HIGH);
    else if (message == "OFF")
      digitalWrite(led,LOW);
  }
}
```
### Python Code:
```python
from Arduino_Master_Delta import *
import time

# To COM 7, Serial write "ON"
writeSerial(7,msg="ON")   # LED turns ON

time.sleep(4)

writeSerial(7,msg="OFF")  # LED turns OFF

time.sleep(1) # Wait 1 second

writeSerial(7,msg="ON")

# However it is not a good idea to use delays in python since it takes time to send data from python to C
# Delays will not be perfect in python.
# The main objective of this module is to collect, filter and visualize data !
```
___
___
## dynamicSerial:
### Arduino Code: (Same code as of ardata example)
```C
const int v = A0;
float Voltage;
String message;

void setup()
{
  Serial.begin(9600);
  pinMode(v,INPUT);
}

void loop()
{
  if(Serial.available()>0) // if anything is available in the serial monitor,
  message = Serial.readString();
  if(message=="Check")
  {
    Voltage = analogRead(v) * 5.0/1023.0 ; // 5/1023 is a conversion unit
    Serial.println(Voltage); // write voltage to serial monitor
  }
}
```
### Python Code:
```python
from Arduino_Master_Delta import *

value1 = dynamicSerial(7,msg="Check")
print(value1) # prints instantaneous voltage at pin A0

value2 = dynamicSerial(7,msg="Check")
print(value2) # prints instantaneous voltage at pin A0
```
___
___
## Hybrids:
![h1](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Hybrids/1.JPG?raw=true)
![h2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Hybrids/2.JPG?raw=true)
![h3](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Hybrids/3.JPG?raw=true)
___
___
## Graph:
### Python Code:
```python
from Arduino_Master_Delta import *

y = [0,1,1,2,2,3,4,5,6,7,8,9,10,10,23]
#x = [2,3,4,5,6,7,8,9,10,11,12,34,43,55,66]

hyb = hybridize(y)

Graph(hyb)  # same as Graph(y=y)
```
![g1](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Graph/G1.jpeg?raw=true)
___
### Python Code:
```python
from Arduino_Master_Delta import *

y = [0,1,1,2,2,3,4,5,6,7,8,9,10,10,23]
x = [2,3,4,5,6,7,8,9,10,11,12,34,43,55,66]

hybridize(x,y)

Graph(hyb)   # same as Graph( x=x, y=y )
```
![g2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Graph/G3.jpeg?raw=true)
___
___
## compGraph:
### Manual:
![man1](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compGraph/Manual_1.JPG?raw=true)


![man2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compGraph/Manual_2.JPG?raw=true)
#### Link to StyleSheet is down here [stylesheet](https://matplotlib.org/3.1.0/gallery/style_sheets/style_sheets_reference.html)
### Using Lists:
![fit1](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compGraph/Fit_List_1.JPG?raw=true)
![fit2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compGraph/Fit_List_2.JPG?raw=true)
![fit3](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compGraph/Fit_List_3.JPG?raw=true)
___
### Using Hybrids:
![hyb1](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compGraph/Fit_Hybrid_1.JPG?raw=true)
![hyb2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compGraph/Fit_Hybrid_2.JPG?raw=true)
___
___
## Graph Attributes:
### Horizontal:
![hor](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/G_attributes/Horizontal.JPG?raw=true)
### Vertical:
![vert](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/G_attributes/Vertical.JPG?raw=true)
### Marker:
![mark](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/G_attributes/Marker.JPG?raw=true)
### Python Code:
![pcCode](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/G_attributes/PC.JPG?raw=true)
### Graph:
![graphForGAttribs](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/G_attributes/G.png?raw=true)
___
___
## Compress:
### Python Code:
![pc](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compress/PC.JPG?raw=true)
### Output:
![op](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compress/OP.JPG?raw=true)
### Graph:
**When we compare them using the compGraph function we can observe the following result which is just an approximation using fit since we're using lists**
![g](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compress/g1.png?raw=true)
### If hybrids are used then, for the same steps we get,
![pc2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compress/pc2.JPG?raw=true)

![g2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/compress/g2.png?raw=true)
___
___
## Frequent:
### Python Code:
![pcmflf](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Frequent/PC.JPG?raw=true)
### Output:
![opmflf](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Frequent/OP.JPG?raw=true)
___
___
# Filter:
## Manual:
![man0](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/man0.JPG?raw=true)
![man1](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/man1.JPG?raw=true)
![man2](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/Manual_2.JPG?raw=true)
___
## Expected:
### Python Code:
![dsfkbskdf](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/PC_expected%5B%5D.JPG?raw=true)
### Graph:
![kdkbd](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/G1_expected%5B%5D.jpeg?raw=true)
___
## expectedType:
### Python Code:
![khdf](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/PC_expectedType.JPG?raw=true)
### Graph:
![sfdnsn](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/G2_expectedType.jpeg?raw=true)
___
## closeTo:
### Python Code:
![lhfkvxn](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/PC_closeTo.JPG?raw=true)
### Graph:
![ksdjfm](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/G3_closeTo.jpeg?raw=true)
___
## farFrom:
### Python Code:
![sjnd](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/PC_farFrom.JPG?raw=true)
### Graph:
![fjhk](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/G6_farFrom.jpeg?raw=true)
___
## farFrom and closeTo together:
### Python Code:
![kdsnbfl](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/PC_Mix.JPG?raw=true)
### Graph:
![kjfsn](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/G7_Mix.jpeg?raw=true)
___
## frequentAverage:
### Python Code:
![dfjhkg](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/PC_frequentAverage.JPG?raw=true)
### Graph:
![iugfdkn](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/G4_frequentAverage.jpeg?raw=true)
___
## limit:
### Python Code:
![jdhfn](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/PC_limit.JPG?raw=true)
### Graph:
![iodwf](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Filter/G5_limit.jpeg?raw=true)
___
___
# Checking Light Intensity using LDR:
## Arduino Code:
![ac](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Light_Intensity/1.JPG?raw=true)
## Python Code:
![pc](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Light_Intensity/2.JPG?raw=true)
## Graph:
![g](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Light_Intensity/3.JPG?raw=true)
___
___
# Impulse Removal:
## Arduino Code:
![kjfhd](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Impulse_Removal/6.JPG?raw=true)
## Python Code:
![ihfgk](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Impulse_Removal/7.JPG?raw=true)
## Graph:
![iuudfsgfu](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Impulse_Removal/4.JPG?raw=true)
___
___
# Impulse Detection:
## Arduino Code:
![sfkljhb](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Impulse%20Detection/6.JPG?raw=true)
## Python Code:
![djkshbf](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Impulse%20Detection/8.JPG?raw=true)
### If you don't know the size of impulses, You can use the following conditions in the filter function instead of closeTo and maxDeviation :
### farFrom='avg' , minDeviation = 5, frequentAverage = True
### farFrom and minDeviation parameters were added by AMD version 0.9.1
## Graph:
![likdjfjghids](https://github.com/SayadPervez/AMD--TUTORIAL/blob/master/Instagram-Uploads/Impulse%20Detection/5.JPG?raw=true)
___
___
Please Star the AMD repository using this link -> [* Me](https://github.com/SayadPervez/Arduino_Master_Delta/stargazers)
