This library includes a Kconfig file for configuring build options on the ESP32. When using the ESP-IDF framework, it is recommended to move library folders to a components folder located in your project's root directory rather than leaving PlatformIO installed libraries in their default location. This is not required but it can result in more a more performant driver. See Using Flash or Disabling Cache for more information.


Using Flash or Disabling Cache

When calling functions that read from or write to flash memory on the ESP32, cache is momentarily disabled and certain interrupts are prevented from firing. This can result in data corruption if the DMX driver is configured improperly.

The included Kconfig file in this library instructs the ESP32's build system to place the DMX driver and some of its functions into IRAM. This and other configuration options can be disabled using the ESP32's menuconfig. The use of menuconfig and Kconfig files is not supported when using the Arduino framework.

The DMX driver can be placed in either IRAM or flash memory. The DMX driver and its associated functions are automatically placed in IRAM to reduce the penalty associated with loading code from flash. Placing the DMX driver in flash is acceptable although less performant. When using the Arduino framework, the DMX driver may be placed in flash.

When the driver is not placed in IRAM, functions which disable the cache will also temporarily disable the DMX driver. To prevent data corruption, it is required to gracefully disable the DMX driver before cache is disabled. This can be done with dmx_driver_disable(). The driver can be reenabled with dmx_driver_enable(). The function dmx_driver_is_enabled() can be used to check the status of the DMX driver.


git submodule add https://github.com/someweisguy/esp_dmx components/esp_dmx

# Modifications to work on ESP-IDF 5.5.1
Some headers were moved/renamed between IDF versions. `dmx/hal/include/uart.h` and `dmx/hal/uart.c` had to be modified.