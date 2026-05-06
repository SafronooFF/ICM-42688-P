# ICM-42688-P
A portable, hardware-independent C library for the ICM-42688-P 6-axis IMU. Designed for embedded systems, it provides a simple API for collecting accelerometer and gyroscope data via SPI.

# Library Installation
Before using the library, move the icm42688.h and icm42688.c files to your project. If you are working with STM32 CubeIDE, move the files to the same directories as shown in the repository.

# API
To use the library, you need to call two functions:
* `setup_function_imu`, responsible for configuring the ICM-42688-P module
* `IMU_data_update`, responsible for updating data
<br>Example function calls:
```C++
int main(void){
...
setup_function_imu();
while(1){
IMU_data_update()
...
}
}
```
Sensor data is automatically processed and stored in the global `IMU_Data` structure named `imu`. To access the latest accelerometer and gyroscope readings, call the `IMU_data_update()` function and then read the corresponding elements of the `imu` structure.
* Accelerometer (measurements in m/s²): `imu.ax`, `imu.ay`, `imu.az`
* Gyroscope (measurements in rad/s): `imu.gx`, `imu.gy`, `imu.gz`
<br>Using data using a complementary filter example
```C++
void update_fltr(void){
uint32_t current_tick = HAL_GetTick();
float dt = (current_tick - last_tick) / 1000.0f;
last_tick = current_tick;
angleX_gyro += imu.gx*dt;
angleY_gyro += imu.gy*dt;
yaw += imu.gz*dt;

float acc_roll = atan2f(imu.ay, imu.az);
float acc_pitch = atan2f(-imu.ax, sqrtf(imu.ay*imu.ay + imu.az*imu.az));
angle_roll = (1.0f - K_DRONE) * (angle_roll + imu.gx * dt) + K_DRONE * acc_roll;
angle_pitch = (1.0f - K_DRONE) * (angle_pitch + imu.gy * dt) + K_DRONE * acc_pitch;
}
```

# Changing library settings for your project
I've set the basic library settings for my project, but you can customize them to suit your needs.
<br> In the `icm42688.h` file, each register and configuration value I use is labeled with corresponding comments and macros. You can also structure the information.
<br>Now you can use the `set_function_in` function, which you must write the appropriate register values ​​and setup information to. This function must be written to `setup_function_imu`.
<br> Example:
```C++
void setup_function_imu(void){
...
//DEVICE_CONFIG
set_function_in(DEVICE_CONFIG_REG, DEVICE_CONFIG_DATA);
...
```
