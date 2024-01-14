# Dattopan

A simple digital "handpan" like synth based on the Electro-Smith Daisy and the MPR121 capacitive touch sensor.

## About

Recently I visited a friend that has a company creating handpans, a modern derivative of the steelpan. So for this project I got inspired to create a kind of digital equivalent using the Daisy and the touch sensor!

## Build

### 1. Clone this repository and pull the libDaisy submodule

```bash
~$ git clone https://github.com/dromer/dattopan.git
~$ cd dattopan
~$ git submodule update --init --recursive
```

### 2.Build the libDaisy toolchain

> **_NOTE:_** libDaisy requires gcc-arm-none-eabi v10.2 !

```bash
~$ cd libdaisy
~$ make
```

### 3. Install `hvcc` and go to the root of this repository to convert the pd patch into C/C++ code

```bash
~$ pip install hvcc
~$ hvcc dattopan.pd -n dattopan -g daisy -m dattopan_hv.json -p heavylib
```

### 4, Go to the source dir

```bash
~$ cd daisy/source/
```

### 5. Install the bootloader

> **_NOTE:_** If you already have the bootloader skip this step and go straight to step 6

Put your daisy into flashing mode (press and hold boot, click reset, release boot) and program the bootloader:

```bash
~$ make program-boot
```

### 6. Edit the header file

Open `HeavyDaisy_dattopan.hpp` and modify these 2 lines:

```cpp
    touch_config.scl = som.GetPin(11);
    touch_config.sda = som.GetPin(12);
```

Either remove them or comment them out:

```cpp
    // touch_config.scl = som.GetPin(11);
    // touch_config.sda = som.GetPin(12);
```

### 7. Build and flash the firmware

Now put your daisy into bootloader mode (click reset, then click boot), compile and upload the dattopan firmware

```bash
~$ make
~$ make program-dfu
```

## Future

- Figure out why the MPR121 doesn't work with time-based effects (delay/reverb) and restore the original patch with dattorro reverb.
- Add controls to be able to change the scale

## Images

![Outside of the box](docs/box.jpg)

![Inside of the box](docs/box_inside.jpg)

![Top of the protoboard](docs/proto_top.jpg)

![Bottom of the protoboard](docs/proto_bottom.jpg)
