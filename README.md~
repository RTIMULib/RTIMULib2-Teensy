# RTIMULib2-Teensy - a versatile 9-dof and 10-dof IMU library for the Teensy3.1

RTIMULib2-Teensy is the simplest way to connect a 9-dof or 10-dof IMU to a Teensy3.1 and obtain fully fused quaternion or Euler angle pose data.

RTIMULib2-Teensy is the development version of the original RTIMULib-Teensy library. So far, the main change is the addition of the runtime magnetometer calibration functionality.

*** Magnetometer calibration is critical for good performance and, with some IMU chips, meaningful fusion results will not be obtained at all unless the magnetometers have been calibrated ***

## Please note that this project is no longer active

This repo is of historical interest only.

## Features

RTIMULib2-Teensy currently supports the following IMUs:

* InvenSense MPU-9150 single chip IMU.
* InvenSense MPU-6050 plus HMC5883 magnetometer on MPU-6050's aux bus (handled by the MPU-9150 driver).
* InvenSense MPU-6050 gyros + acclerometers. Treated as MPU-9150 without magnetometers.
* InvenSense MPU-9250 single chip IMU (I2C and SPI)
* STM LSM9DS0 single chip IMU
* L3GD20H + LSM303D (optionally with the LPS25H) as used on the Pololu AltIMU-10 v4.
* L3GD20 + LSM303DLHC as used on the Adafruit 9-dof (older version with GD20 gyro) IMU. 
* L3GD20H + LSM303DLHC (optionally with BMP180) as used on the new Adafruit 10-dof IMU.
* Bosch BMX055 (although magnetometer support is experimental currently).
* Bosch BNO055 with onchip fusion.

Pressure/temperature sensing is supported for the following pressure sensors:

* BMP180
* LPS25H
* MS5611
* MS5637

Note that currently only pressure sensors connected via I2C are supported.  Also, an MS5637 sensor will be auto-detected as an MS5611. To get the correct processing for the MS5637, edit the RTIMULib.ini file and set PressureType=5.

By default, RTIMULib2-Teensy will try to auto-discover IMUs and pressure sensors on I2C and SPI busses (only IMUs on the SPI bus). By default, the SPI IMU interface uses Teensy3.1 pin 9 as the chip select but this can be changed by editing libraries/RTIMULib/RTIMUSettings.h.

RTIMULib2-Teensy also supports multiple sensor integration fusion filters such as Kalman filters. Note that the BNO055 always uses its onchip fusion results rather than any of the RTIMULib2-Teensy filters. Also, if performs its own magnetometer calibration so normal calibration data is not used.

If an SD card is available on the Teensy3.1, RTIMULib2-Teensy will use it for configuration data. This uses the SPI interface and pin 10 as select by default. This can be changed by editing libraries/RTIMULib/RTIMUSettings.h. Configuration will be stored in a file called RTIMULib.ini on the SD card. This can be edited by hand (on another machine with an SD card reader) if there is any need to change defaults or auto-detection settings. A simple sketch is provided that deletes this file if necessary - changing IMU type would be an example. The RTIMULib.ini file could be edited but it's quicker to just delete the ini file and start again if the IMU type is changed.

If no SD card is available, EEPROM is used just to save magnetometer calibration data. If other settings need to be changed (such as sample rate), that should be done by changing values in the RTIMUSettings structure between the creation of the settings object and using it in a call to RTIMULib::createIMU().

The actual RTIMULib and support libraries are in the library directory. The other top level directories contain example sketches.

*** Important note ***
It is essential to calibrate the magnetometers or else very poor results will obtained, especially with the MPU-9150 and MPU-9250. If odd results are being obtained, suspect the magnetometer calibration! Operating without calibration can be done but is not recommended.

## Note about magnetometer (compass) calibration

For many IMUs, fused data may be completely unusable unless the magnetometers have been calibrated.

RTIMULib2-Teensy has two mechanisms that can be used to calibrate the magnetometers:

* Manual calibration. This is where TeensyMagCal has been used to set magnetometer calibration data. Once this is done, there should be no need to repeat calibration unless the magnetic environment changes. 

* Runtime calibration. This mechanism is used if there is no manual calibration data. The magnetometers will remain uncalibrated until a sufficient range of readings has been obtained in each of the axes. The code will continue to monitor magnetometer readings for new maxima and minima and update the calibration data as required. This data is not saved so the procedure will start from scratch if the code is restarted.

Also, if using a non-standard axis rotation, magnetometer calibration MUST be run AFTER changing the axis rotation.

## The Example Sketches

### Build and run

To build and run the example sketches, start the Teensyduino IDE and use File --> Preferences and then set the sketchbook location to:

	.../RTIMULib2-Teensy

where "..." represents the path to the RTIMULib2-Teensy directory. The directory is set up so that there's no need to copy the libraries into the main Arduino libraries directory although this can be done if desired.

### TeensyMagCal

This sketch can be used to calibrate the magnetometers and should be run before trying to generate fused pose data. It also needs to be rerun at any time that the configuration is changed (such as different IMU or different IMU reference orientation). Load the sketch and waggle the IMU around, making sure all axes reach their minima and maxima. The display will stop updating when this occurs. Then, enter 's' followed by enter into the IDE serial monitor to save the data.

### TeensyIMU

TeensyIMU is the main demo sketch. It configures the IMU based on settings in RTIMUSettings.cpp. Change these to alter any of the parameters or edit the RTIMULib.ini file after auto-detection.


### TeensyIMU10

This is exactly the same as TeensyIMU except that it adds support for a pressure sensor. If a pressure sensor was found during auto-detection, this will be used by the sketch. If not pressure sensors were found, the sketch will report this and stop.


### TeensyAccel

This is similar to TeensyIMU except that it subtracts the rotated gravity vector from the accelerometer outputs in order to obtain the residual accelerations - i.e. those not attributable to gravity.

### TeensyDeleteEEPROM

A simple sketch that can be downloaded to clear mag calibration data from the EEPROM. This is useful if it is desired to revert to runtime magnetometer calibration.

### TeensyDeleteIni

A simple sketch that can be downloaded to delete the RTIMULib.ini file from the SD card.

