# ICM-42688-P
A portable, hardware-independent C library for the ICM-42688-P 6-axis IMU. Designed for embedded systems, it provides a clean API for accelerometer and gyroscope data acquisition via SPI.

# API
To use the library, you need to call two functions:
* `setup_function_imu` responsible for setting up
* `IMU_data_update` responsible for updating the data
example:
```C++
int main(void){
...
setup_function_imu();
  while(1){
    IMU_data_update()
  }
}
```
Sensor data is automatically processed and stored in a global `IMU_Data` structure named `imu`. To access the latest accelerometer and gyroscope readings, call the `IMU_data_update()` function and then read the corresponding members of the imu struct.
* Accelerometer (m/s²): `imu.ax`, `imu.ay`, `imu.az`
* Gyroscope (rad/s): `imu.gx`, `imu.gy`, `imu.gz`
<br>example of data usage
```C++
void update_fltr(void){
  uint32_t current_tick = HAL_GetTick();
  float dt = (current_tick - last_tick) / 1000.0f;
  last_tick = current_tick;
  angleX_gyro += imu.gx*dt;
  angleY_gyro += imu.gy*dt;
  yaw += imu.gz*dt;

  float acc_roll  = atan2f(imu.ay, imu.az);
  float acc_pitch = atan2f(-imu.ax, sqrtf(imu.ay*imu.ay + imu.az*imu.az));
  angle_roll  = (1.0f - K_DRONE) * (angle_roll  + imu.gx * dt) + K_DRONE * acc_roll;
  angle_pitch = (1.0f - K_DRONE) * (angle_pitch + imu.gy * dt) + K_DRONE * acc_pitch;
}
```
