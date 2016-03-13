==========
stm32tools
==========

Helper tools that I use when developing software for STM32 MCUs. The
tools are mostly based on OpenOCD, with some additional scripts
collected from various projects.

License
=======

MIT or other for borrowed scripts (see specific files for info).

Tools
=====

{reset,debug,flash}-dev
-----------------------

Thelper tools that either reset the target device, upload firmware to
the internal flash, or just start an instance of OpenOCD waiting for
connection from `gdb`.

The `reset-dev` script should be able to bring out the MCU from sleep
mode.

The `flash-dev` script will upload the firmware to the device, writing
it to default flash location for given target. If your device has 2
flash banks or you want to override the location the firwmare gets
written to, edit `board.cfg` to match your setup.

The `debug-dev` script will halt the MCU and wait for `gdb` to
establish a connection. If you pass a firmware file as parameter, the
script will flash the device, reset it and halt.

Usage::

  # reset a device
  ./reset-dev

  # halt the target, if firmware-path is provided, flash it and reset
  # before
  debug-dev [<firmware-path>]
  # start gdb, and run
  (gdb) target remote localhost:3333

  # write firmware to the device
  flash-dev <firmware-path>

Also, if your MCU is other than STM32F1, edit `board.cfg` and select
proper target.

The `transport.cfg` script tries to guess the version of STLink by
parsing the output of `lsusb`. If that fails, you may need to provide
your own `transport.cfg`.

checkstack
----------

The tool scans the output of objdump to find which functions have a
larger than usual stack size. Usage (assuming ARM architecure)::

  arm-none-eabi-objdump -d <binary.elf> | checkstack.pl arm 20

This will print all functions with stack larger than 20 bytes.

udev
----

Copy `udev/99-stlink.rules` to `/etc/udev/rules.d` to make sure that
the STLink device is writable by regular user.
