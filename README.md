# Cayenne Low Power Payload
Payload builder library


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
