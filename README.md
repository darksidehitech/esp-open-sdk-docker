Docker image for esp-open-sdk toolchain
=========================================


## Pulling:

  ```
  docker pull darksidehitech/esp-open-sdk
  ```

## Running:

So far this container was used for building
[esp-open-rtos](https://github.com/SuperHouse/esp-open-rtos) projects. For example:

  ```
  git clone --recursive https://github.com/Superhouse/esp-open-rtos.git
  cd esp-open-rtos
  docker run -it --device=/dev/ttyUSB0 -v ${PWD}:/home/build \
      darksidehitech/esp-open-sdk make flash -C examples/blink ESPPORT=/dev/ttyUSB0
  ```

If there exist access errors such as:

  ```
  serial.serialutil.SerialException: [Errno 13] could not open port /dev/ttyUSB0: [Errno 13] Permission denied: '/dev/ttyUSB0'
  ../../common.mk:247: recipe for target 'flash' failed
  make: *** [flash] Error 1
  make: Leaving directory '/home/build/examples/blink'
  ```

You need to provide `udev` rules for your `tty` device, for example:

  ```
  echo 'SUBSYSTEM =="tty", ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea60" , MODE="0666"' \
    | sudo tee /etc/udev/rules.d/52-esp-serial.rules
  ```
