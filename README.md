# ICM-42688-P
A portable, hardware-independent C library for the ICM-42688-P 6-axis IMU. Designed for embedded systems, it provides a clean API for accelerometer and gyroscope data acquisition via SPI.

# API
To use the library, you need to call two functions:
* `setup_function_imu` responsible for setting up
* `IMU_data_update responsible` for updating the data
example:
```C++
int main(void){
...
setup_function_imu();
  while(1){
    IMU_data_update responsible()
  }
}
```
