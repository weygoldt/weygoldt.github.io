---
title: "PlatformIO: Hardware dev from the command line"
summary: "An introduction to PlatformIO, a tool that makes software development for microcontrollers easier."
showSummary: true
categories: ["Post"]
tags: ["arduino", "hardware", "linux"]
date: 2023-05-01T14:51:09+02:00
draft: false
---

If you've worked with Arduinos before, you know that beeing constrained to the Arduino IDE can be quite limiting. PlatformIO is a cross-platform, cross-architecture, and multiple framework tool that can help with this. It is designed to make hardware development more comfortable and more efficient.

PlatformIO is typically advertised as a VSCode plugin, which turns VSCode into an Arduino IDE-like platform. However, there is a lesser-known feature that developers can take advantage of - the simple command-line toolbox. In this blog post, we will walk you through how to install and use PlatformIO via the command line.

To begin, PlatformIO is a Python package, so it can be installed from PyPI, including within a virtual environment. However, it is highly recommended to use PlatformIO's installation script instead. Here's how you can install PlatformIO using the installation script:

```sh
curl -fsSL https://raw.githubusercontent.com/platformio/platformio-core-installer/master/get-platformio.py -o get-platformio.py
python3 get-platformio.py
```

During installation, PlatformIO creates its virtual environment, which is beneficial since it doesn't interfere with system packages. However, the standard shell doesn't recognize the PlatformIO executables, so we need to create symbolic links. You can do that using the following commands:

```sh
ln -s ~/.platformio/penv/bin/platformio ~/.local/bin/platformio
ln -s ~/.platformio/penv/bin/pio ~/.local/bin/pio
ln -s ~/.platformio/penv/bin/piodebuggdb ~/.local/bin/piodebuggdb
```

On a Linux system, you also need to add udev rules. This snippet downloads the rules, places them in the appropriate location, reloads the udev service, and adds the current user to the relevant groups:

```sh
curl -fsSL https://raw.githubusercontent.com/platformio/platformio-core/develop/platformio/assets/system/99-platformio-udev.rules | sudo tee /etc/udev/rules.d/99-platformio-udev.rules
```

To restart udev on debian-based distributions, run:

```sh
sudo service udev restart
sudo usermod -a -G dialout $USER
sudo usermod -a -G plugdev $USER
```

On arch-based distributions, use the following instead:

```sh
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo usermod -a -G uucp $USER
sudo usermod -a -G lock $USER
```

Log out and log back in for the changes to take effect.

By default, PlatformIO enables telemetry. However, you can disable it by running:

```sh
pio settings set enable_telemetry No
pio settings get
```

Next, you can initialize `pio` in your directory by creating an example project directory, for example, `mkdir pio_test && cd pio_test`. Then, list all the available boards (platforms) by running `pio boards`. Choose your board and run:

```sh
pio project init --ide vim --board teensy41
```

Now you can edit the source code using your editor of choice. To test if the code compiles, you can simply run:

```sh
pio run
```

To flash an attached board, add the following:

```sh
pio run --target upload
```

For convenience, I personally created shell aliases `comp` for compilation and `flash` to upload. If you let your device access the serial port to print some debug information, you can monitor this by running:

```sh
pio device
```

In conclusion, PlatformIO is a powerful tool for hardware development that provides a flexible and efficient workflow for software developers working with hardware. With its support for multiple platforms, frameworks, and libraries, along with a command-line interface and integrated debugging, PlatformIO is an excellent choice for anyone looking to streamline their hardware development workflow.

Photo by <a href="https://unsplash.com/@vishnumaiea?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Vishnu Mohanan</a> on <a href="https://unsplash.com/photos/pfR18JNEMv8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
