..  Copyright (c) 2014-present PlatformIO <contact@platformio.org>
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
       http://www.apache.org/licenses/LICENSE-2.0
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

Configuration
-------------

.. contents::
    :local:

CPU Frequency
~~~~~~~~~~~~~

See :ref:`projectconf_board_f_cpu` option from :ref:`projectconf`

.. code-block:: ini

    [env:myenv]
    ; set frequency to 160MHz
    board_f_cpu = 160000000L

FLASH Frequency
~~~~~~~~~~~~~~~

See :ref:`projectconf_board_f_flash` option from :ref:`projectconf`. Possible
values:

* ``40000000L`` (default)
* ``80000000L``

.. code-block:: ini

    [env:myenv]
    ; set frequency to 80MHz
    board_f_flash = 80000000L

FLASH Mode
~~~~~~~~~~

Flash chip interface mode. This parameter is stored in the binary image
header, along with the flash size and flash frequency. The ROM bootloader
in the ESP chip uses the value of these parameters in order to know how to
talk to the flash chip.

See :ref:`projectconf_board_flash_mode` option from :ref:`projectconf`. Possible
values:

* ``qio``
* ``qout``
* ``dio``
* ``dout``

.. code-block:: ini

    [env:myenv]
    board_flash_mode = qio

Debug Level
~~~~~~~~~~~

Please use one of the next :ref:`projectconf_build_flags` to change debug level.
A :ref:`projectconf_build_flags` option could be used only the one time per
build environment. If you need to specify more flags, please separate them
with a new line or space.

Actual information is available in `Arduino for ESP32 Board Manifest <https://github.com/espressif/arduino-esp32/blob/master/boards.txt#L80>`_.
Please scroll to ``esp32.menu.DebugLevel`` section.


.. code-block:: ini

    [env:myenv]
    platform = ...
    board = ...
    framework = arduino

    ;;;;; Possible options ;;;;;;

    ; None
    build_flags = -DCORE_DEBUG_LEVEL=0

    ; Error
    build_flags = -DCORE_DEBUG_LEVEL=1

    ; Warn
    build_flags = -DCORE_DEBUG_LEVEL=2

    ; Info
    build_flags = -DCORE_DEBUG_LEVEL=3

    ; Debug
    build_flags = -DCORE_DEBUG_LEVEL=4

    ; Verbose
    build_flags = -DCORE_DEBUG_LEVEL=5

Upload Speed
~~~~~~~~~~~~

You can set custom upload speed using  :ref:`projectconf_upload_speed` option
from :ref:`projectconf`

.. code-block:: ini

    [env:myenv]
    upload_speed = 9600

Over-the-Air (OTA) update
~~~~~~~~~~~~~~~~~~~~~~~~~

Demo code for:

* `Arduino <https://github.com/espressif/arduino-esp32/tree/master/libraries/ArduinoOTA/examples/BasicOTA>`_
* `ESP-IDF <https://github.com/espressif/esp-idf/tree/master/examples/system/ota>`_

There are 2 options how to upload firmware OTA:

* Directly specify :option:`platformio run --upload-port` in command line

.. code-block:: bash

    platformio run --target upload --upload-port IP_ADDRESS_HERE or mDNS_NAME.local

* Specify ``upload_port`` option in :ref:`projectconf`

.. code-block:: ini

   [env:myenv]
   upload_port = IP_ADDRESS_HERE or mDNS_NAME.local

For example,

* ``platformio run -t upload --upload-port 192.168.0.255``
* ``platformio run -t upload --upload-port myesp32.local``

Authentication and upload options
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can pass additional options/flags to OTA uploader using
``upload_flags`` option in :ref:`projectconf`

.. code-block:: ini

    [env:myenv]
    upload_flags = --port=3232

Available flags

* ``--port=ESP_PORT`` ESP32 OTA Port. Default 3232
* ``--auth=AUTH`` Set authentication password
* ``--spiffs`` Use this option to transmit a SPIFFS image and do not flash
  the module

For the full list with available options please run

.. code-block:: bash

    ~/.platformio/packages/tool-espotapy/espota.py -h

    Usage: espota.py [options]

    Transmit image over the air to the esp32 module with OTA support.

    Options:
      -h, --help            show this help message and exit

      Destination:
        -i ESP_IP, --ip=ESP_IP
                            ESP32 IP Address.
        -p ESP_PORT, --port=ESP_PORT
                            ESP32 ota Port. Default 3232

      Authentication:
        -a AUTH, --auth=AUTH
                            Set authentication password.

      Image:
        -f FILE, --file=FILE
                            Image file.
        -s, --spiffs        Use this option to transmit a SPIFFS image and do not
                            flash the module.

      Output:
        -d, --debug         Show debug output. And override loglevel with debug.
        -r, --progress      Show progress output. Does not work for ArduinoIDE

Using Arduino Framework with Staging version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

PlatformIO will install the latest Arduino Core for ESP32 from
https://github.com/espressif/arduino-esp32. The `Git <https://git-scm.com>`_
should be installed in a system. To update Arduino Core to the latest revision,
please open :ref:`pioide` and navigate to ``PIO Home > Platforms > Updates``.

1.  Please install :ref:`pioide`
2.  Initialize a new project, open :ref:`projectconf` and set
    :ref:`projectconf_env_platform` to
    ``https://github.com/platformio/platform-espressif32.git#feature/stage``.
    For example,

    .. code-block:: ini

        [env:esp32dev]
        platform = https://github.com/platformio/platform-espressif32.git#feature/stage
        board = esp32dev
        framework = arduino

3.  Try to build project
4.  If you see build errors, then try to build this project using the same
    ``stage`` with Arduino IDE
5.  If it works with Arduino IDE but doesn't work with PlatformIO, then please
    `file new issue <https://github.com/platformio/platform-espressif32/issuess>`_
    with attached information:

    - test project/files
    - detailed log of build process from Arduino IDE (please copy it from
      console to https://hastebin.com)
    - detailed log of build process from PlatformIO Build System (please copy
      it from console to https://hastebin.com)

Arduino Core Wiki
~~~~~~~~~~~~~~~~~

Tips, tricks and common problems: http://desire.giesecke.tk/index.php/2018/01/30/esp32-wiki-entries/
