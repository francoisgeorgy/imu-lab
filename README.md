# imu-lab
    
    git submodule add https://github.com/francoisgeorgy/micropython-fusion
    git submodule add https://github.com/francoisgeorgy/micropython-mpu9x50
    git submodule add https://github.com/francoisgeorgy/BNO08x_Pico_Library
    git submodule add https://github.com/francoisgeorgy/Kalman-and-Bayesian-Filters-in-Python 
    git submodule add https://github.com/francoisgeorgy/Adafruit_BNO08x         C,  SPI, I2C, UART
    git submodule add https://github.com/francoisgeorgy/Adafruit_CircuitPython_BNO08x 
    git submodule add https://github.com/francoisgeorgy/MicroPython_LIS3DH
    git submodule add https://github.com/francoisgeorgy/esp32_bno08x_driver     C,   freertos, SPI, july 2024
    git submodule add https://github.com/myles-parfeniuk/esp32_BNO08x           C++, freertos, SPI, dec 2024
    git submodule add https://github.com/francoisgeorgy/FastIMU
    git submodule add https://github.com/CarbonAeronautics/Part-XV-1DKalmanFilter
    git submodule add https://github.com/RPi-Distro/RTIMULib.git
    git submodule add https://github.com/bjohnsonfl/Madgwick_Filter
    git submodule add https://github.com/itsahmedkhalil/MobileRobotEKF-LQR
    git submodule add https://github.com/laitathei/mpu9250_ahrs
    git submodule add https://github.com/mattzzw/Arduino-mpu6050
    git submodule add https://github.com/memsindustrygroup/Open-Source-Sensor-Fusion
    git submodule add https://github.com/morgil/madgwick_py
    git submodule add https://github.com/nihalpasham/micropython_sensorfusion 
    git submodule add https://github.com/ozzmaker/BerryIMU
    git submodule add https://github.com/pms67/Attitude-Estimation 
    git submodule add https://github.com/xioTechnologies/Fusion (sedgwick)

# Notes

## FreeRTOS

[xEventGroupWaitBits](https://www.freertos.org/Documentation/02-Kernel/04-API-references/12-Event-groups-or-flags/04-xEventGroupWaitBits)
Read bits within an RTOS event group, optionally entering the Blocked state (with a timeout) to wait for a bit or group of bits to become set.


# Implementation notes

## Reset 

    static void hal_hardwareReset(void) {
        if (_reset_pin != -1) {
            gpio_init(_reset_pin);
            gpio_set_dir(_reset_pin, GPIO_OUT);
            gpio_put(_reset_pin, 1);
            sleep_ms(10);
            gpio_put(_reset_pin, 0);
            sleep_ms(10);
            gpio_put(_reset_pin, 1);
            sleep_ms(10);
        }
    }

    def hard_reset(self):
        """Hardware reset the sensor to an initial unconfigured state"""
        self._reset.direction = Direction.OUTPUT
        self._int.direction = Direction.INPUT
        self._int.pull = Pull.UP
        print("Hard resetting...")
        self._reset.value = True  # perform hardware reset
        time.sleep(0.01)
        self._reset.value = False
        time.sleep(0.01)
        self._reset.value = True
        self._wait_for_int()
        print("Done!")
        self._read_packet()
