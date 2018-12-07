## Cayenne Low Power Payload

### Overview

The Cayenne Low Power Payload (LPP) provides a convenient and easy way to send data over LPWAN networks such as LoRaWAN.  The Cayenne LPP is compliant with the payload size restriction, which can be lowered down to 11 bytes, and allows the device to send multiple sensor data at one time.  

Additionally, the Cayenne LPP allows the device to send different sensor data in different frames. In order to do that, each sensor data must be prefixed with two bytes:

- **Data Channel:** Uniquely identifies each sensor in the device across frames, eg. “indoor sensor”
- **Data Type:** Identifies the data type in the frame, eg. “temperature”.

### Payload structure
<table style="width: 100%;">
<tbody>
<tr>
<td style="font-size: 15px; padding: 10px;"><b>1 Byte</b></td>
<td style="font-size: 15px; padding: 10px;"><b>1 Byte</b></td>
<td style="font-size: 15px; padding: 10px;"><b>N Bytes</b></td>
<td style="font-size: 15px; padding: 10px;"><b>1 Byte</b></td>
<td style="font-size: 15px; padding: 10px;"><b>1 Byte</b></td>
<td style="font-size: 15px; padding: 10px;"><b>M Bytes</b></td>
<td style="font-size: 15px; padding: 10px;"><b> ... </b></td>
</tr>
<tr>
<td>Data1 Ch.</td>
<td>Data1 Type</td>
<td>Data1</td>
<td>Data2 Ch.</td>
<td>Data2 Type</td>
<td>Data2</td>
<td>...</td>
</tr>
</tbody>
</table>

### Data Types

Data Types conform to the IPSO Alliance Smart Objects Guidelines, which identifies each data type with an “Object ID”.  However, as shown below, a conversion is made to fit the Object ID into a single byte.

```
LPP_DATA_TYPE = IPSO_OBJECT_ID - 3200
```

Each data type can use 1 or more bytes to send the data according to the following table.

<table style="width: 100%;">
<tbody>
<tr>
<td style="font-size: 15px; padding: 10px;"><b>Type</b></td>
<td style="font-size: 15px; padding: 10px;"><b>IPSO</b></td>
<td style="font-size: 15px; padding: 10px;"><b>LPP</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Hex</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Data Size</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Data Resolution per bit</b></td>
</tr>
<tr>
<td>Digital Input</td>
<td>3200</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>1</td>
</tr>
<tr>
<td>Digital Output</td>
<td>3201</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
<tr>
<td>Analog Input</td>
<td>3202</td>
<td>2</td>
<td>2</td>
<td>2</td>
<td>0.01 Signed</td>
</tr>
<tr>
<td>Analog Output</td>
<td>3203</td>
<td>3</td>
<td>3</td>
<td>2</td>
<td>0.01 Signed</td>
</tr>
<tr>
<td>Illuminance Sensor</td>
<td>3301</td>
<td>101</td>
<td>65</td>
<td>2</td>
<td>1 Lux Unsigned MSB</td>
</tr>
<tr>
<td>Presence Sensor</td>
<td>3302</td>
<td>102</td>
<td>66</td>
<td>1</td>
<td>1</td>
</tr>
<tr>
<td>Temperature Sensor</td>
<td>3303</td>
<td>103</td>
<td>67</td>
<td>2</td>
<td>0.1 °C Signed MSB</td>
</tr>
<tr>
<td>Humidity Sensor</td>
<td>3304</td>
<td>104</td>
<td>68</td>
<td>1</td>
<td>0.5 % Unsigned</td>
</tr>
<tr>
<td>Accelerometer</td>
<td>3313</td>
<td>113</td>
<td>71</td>
<td>6</td>
<td>0.001 G Signed MSB per axis</td>
</tr>
<tr>
<td>Barometer</td>
<td>3315</td>
<td>115</td>
<td>73</td>
<td>2</td>
<td>0.1 hPa Unsigned MSB</td>
</tr>
<tr>
<td>Gyrometer</td>
<td>3334</td>
<td>134</td>
<td>86</td>
<td>6</td>
<td>0.01 °/s Signed MSB per axis</td>
</tr>
<tr>
<td rowspan="3">GPS Location</td>
<td rowspan="3">3336</td>
<td rowspan="3">136</td>
<td rowspan="3">88</td>
<td rowspan="3">9</td>
<td>Latitude : 0.0001 ° Signed MSB</td>
</tr>
<tr>
<td>Longitude : 0.0001 ° Signed MSB</td>
</tr>
<tr>
<td>Altitude : 0.01 meter Signed MSB</td>
</tr>
</tbody>
</table>

### Examples

#### Device with 2 temperature sensors

<table style="width: 100%;">
<tbody>
<tr>
<td style="font-size: 15px; padding: 10px;"><b>Payload (Hex)</b></td>
<td style="font-size: 15px; padding: 10px;" colspan="2">03 67 01 10 05 67 00 FF</td>
</tr>
<tr>
<td style="font-size: 15px; padding: 10px;"><b>Data Channel</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Type</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Value</b></td>
</tr>
<tr>
<td>03 ⇒ 3</td>
<td>67 ⇒ Temperature</td>
<td>0110 = 272 ⇒ 27.2°C</td>
</tr>
<tr>
<td>05 ⇒ 5</td>
<td>67 ⇒ Temperature</td>
<td>00FF = 255 ⇒ 25.5°C</td>
</tr>
</tbody>
</table>

#### Device with temperature and acceleration sensors

**Frame N**
<table style="width: 100%;">
<tbody>
<tr>
<td style="font-size: 15px; padding: 10px;"><b>Payload (Hex)</b></td>
<td style="font-size: 15px; padding: 10px;" colspan="2">01 67 FF D7</td>
</tr>
<tr>
<td style="font-size: 15px; padding: 10px;"><b>Data Channel</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Type</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Value</b></td>
</tr>
<tr>
<td>01 ⇒ 1</td>
<td>67 ⇒ Temperature</td>
<td>FFD7 = -41 ⇒ -4.1°C</td>
</tr>
</tbody>
</table>

**Frame N+1**
<table style="width: 100%;">
<tbody>
<tr>
<td style="font-size: 15px; padding: 10px;"><b>Payload (Hex)</b></td>
<td style="font-size: 15px; padding: 10px;" colspan="2">06 71 04 D2 <i>FB 2E</i> <i>00 00</i></td>
</tr>
<tr>
<td style="font-size: 15px; padding: 10px;"><b>Data Channel</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Type</b></td>
<td style="font-size: 15px; padding: 10px;"><b>Value</b></td>
</tr>
<tr>
<td rowspan="3">06 ⇒ 6</td>
<td rowspan="3">71 ⇒ Accelerometer</td>
<td>X: 04D2 = +1234 ⇒ +1.234G</td>
</tr>
<tr>
<td><i>Y: FB2E = -1234 ⇒ -1.234G</i></td>
</tr>
<tr>
<td><i>Z: 0000 = 0 ⇒ 0G</i></td>
</tr>
</tbody>
</table>

# Example use
```
#include 'CayenneLPP.h';

#define MAX_SIZE 200; // depends on spreading factor and frequency used

CayenneLPP Payload(MAX_SIZE);

float celsius = -4.1;
float accel[] = {1.234, -1.234, 0};
float rh = 30;
float hpa = 1014.1;
float latitude = 42.3519;
float longitude = -87.9094;
float altitude:10

int size = 0;

Payload.reset();
size = Payload.addTemperature(0, celsius);

if (size == 0) {
    // not enough byte left to add the data
}

else {
    // add function returned current payload size
}


Payload.addAccelerometer(1, accel[0], accel[1], accel[2]);
Payload.addRelativeHumidity(3, rh);
Payload.addBarometricPressure(4, hpa);
Payload.addGPS(5, latitude, longitude, altitude);

// Call LoRaWAN library to send the frame
LORA_SEND(Payload.getBuffer(), Payload.getSize());
```
