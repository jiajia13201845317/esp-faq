Security
========

:link_to_translation:`zh_CN:[中文]`

.. raw:: html

   <style>
   body {counter-reset: h2}
     h2 {counter-reset: h3}
     h2:before {counter-increment: h2; content: counter(h2) ". "}
     h3:before {counter-increment: h3; content: counter(h2) "." counter(h3) ". "}
     h2.nocount:before, h3.nocount:before, { content: ""; counter-increment: none }
   </style>

--------------

Is the firmware in ESP8266 readable?
---------------------------------------------------------

  Yes, because the firmware in ESP8266 is located in the external flash, thus can be read externally. In addition, ESP8266 does not support flash encryption and all the data is written in plaintext.

--------------

Is it possible to encrypt firmware for ESP8285?
--------------------------------------------------------------

  - No, the ESP8285 chip does not support firmware encryption function.
  - Both ESP32 and ESP32-S2 support firmware encryption, thus can be your substitution.
  - If you insist on using ESP8285, you can achieve data encryption by adding an encrypted chip externally.

--------------

What is the difference between secure boot v1 and v2?
------------------------------------------------------
  
  Compared with secure boot v1, secure boot v2 has the following improvements:
  - The bootloader and app use the same signature format.
  - The bootloader and app use the same signing key.

  Currently, `secure boot v1 <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v1.html>`_ is only reommended for earlier versions than ESP32 v3.0. For ESP32 v3.0 and later versions, ESP32-C3, ESP32-S2, and ESP32-S3, it is recommended to use `secure boot v2 <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v2.html>`_.

--------------

After enabling secure boot, there is a build error indicating missing files. What could be the reasons？
-------------------------------------------------------------------------------------------------------------------------------

  Error log: /Makefile.projbuild:7/f/ESP32Root/secure_boot_signing_key.pem

  Reason: security boot is a function for firmware signature verification, which requires generating key pairs.
  - For the method of generating a key pair when secure boot v1 is enabled, please refer to `secure boot v1 key generation <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v1.html#generating-secure-boot-signing-key>`_.
  - For the method of generating a key pair when secure boot v2 is enabled, please refer to `secure boot v2 key generation <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v2.html#generating-secure-boot-signing-key>`_.

--------------

After enabling secure boot, is it possible for modules to be flashed again?
-------------------------------------------------------------------------------------------------

  - If the secure boot v1 is configured as one-time, then it can only be flashed once and the bootloader firmware cannot be reflashed.
  - If the secure boot v1 is configured as reflashable, then the bootloader firmware can be flashed again.
  - The secure boot v2 allows reflashing the bootloader and app firmware.

--------------

With flash encryption enabled, a module reports an error as ``flash read error`` after reflashed. How to resolve such issue?
---------------------------------------------------------------------------------------------------------------------------------------------------

  With flash encryption enabled, the module will not support plaintext firmware flash. For common failures, please refer to `Possible Failures <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/flash-encryption.html#id9>`_. You can use the `espefuse <https://docs.espressif.com/projects/esptool/en/latest/esp32/espefuse/index.html>`_ script to disable the encryption and then reflash the plaintext firmware, or directly flash the encrypted firmware to devices referring to the `flash encryption example <https://github.com/espressif/esp-idf/tree/master/examples/security/flash_encryption>`_.
  
  .. note::
      
      Please note there is a time limit for the flash encrypted function.

--------------

After enabling flash encryption and secure boot for ESP32, how to disable them?
-------------------------------------------------------------------------------------------------

  - If you are using the one-time flash (Release) mode, both flash encryption and secure boot cannot be disabled.
  - If you are using the reflashable (Development (NOT SECURE)) mode, the flash encryption can be disabled, please refer to `Disabling Flash Encryption <https://docs.espressif.com/projects/esp-idf/en/v4.4.2/esp32/security/flash-encryption.html#disabling-flash-encryption>`_; while the secure boot cannot be disabled.

--------------

Is there any security strategy for ESP32 to protect its firmware?
-----------------------------------------------------------------------------------

  - ESP32 supports flash encryption and secure boot.
  - For flash encryption, please refer to `flash encryption <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/flash-encryption.html>`_.
  - For secure boot, please refer to `secure boot <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v1.html>`_.
  - For secure boot V2, please refer to `secure boot V2 for chip revision v3.0 <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v2.html>`_.

--------------

When ESP32 debugging GDB after enabling flash encryption, why does it continuously reset and restart?
---------------------------------------------------------------------------------------------------------------------------------

  - After ESP32 enabling flash encryption or secure boot, it will restrict JTAG debugging by default, please refer to `Tips and Quirks <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/jtag-debugging/tips-and-quirks.html#jtag-with-flash-encryption-or-secure-boot>`_.
  - You can read the current JTAG status of your chip using the ``espefuse.py summary`` command from esptool.

------------------

How to enable flash encryption for ESP32?
----------------------------------------------------------------------------------------------------------------------------------------

  - It can be enabled via menuconfig or idf.py menuconfig by configuring ``Security features`` -> ``Enable flash encryption on boot (READ DOCS FIRST)``.
  - Please refer to `Flash encryption instructions <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/flash-encryption.html#flash>`_.
  
------------------

After GPIO0 is pulled down, the ESP32 cannot enter download mode and prints "download mode is disable". What could be the reason?
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - The log means the chip's UART Download mode has been disabled. You can check this via the ``UART_DOWNLOAD_DIS`` bit in `eFuse <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/system/efuse.html?highlight=download%20mode>`_.
  - Please note that after the Production mode of flash encryption is enabled, the UART Download mode will be disabled by default. For more information, please refer to `UART ROM download mode <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/kconfig.html#config-secure-uart-rom-dl-mode>`_.
  
-----------------------

Can the secure boot function be enabled for ESP32 in Arduino development environment?
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - No. If you want to use Arduino for development, the only way to enable the secure boot function is to use Arduino as an IDF component.

------------

What are the use scenarios for secure boot and flash encryption?
--------------------------------------------------------------------

  - When secure boot is enabled, the device will only load and run firmware that is signed by the specified key. Therefore, it can prevent the device from loading illegal firmware and prevent unauthorized firmware from being flashed to the device.
  - When flash encryption is enabled, the partitions on the flash where firmware is stored and the data in the partitions marked as "encrypeted" will be encrypted. Therefore, it can prevent the data from being illegally viewed, and firmware data copied from flash cannot be applied to other devices.

------------

What are the data stored in eFuse involved in secure boot and flash encryption?
----------------------------------------------------------------------------------

  - For the data stored in eFuse used in secure boot v1, please refer to `secure boot v1 efuses <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v1.html#background>`_。
  - For the data stored in eFuse used in secure boot v2, please refer to `secure boot v2 efuses <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/secure-boot-v2.html#efuse-usage>`_。
  - For the data stored in eFuse used in flash encryption, please refer to `flash encryption efuses <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/security/flash-encryption.html#relevant-efuses>`_。

------------

Enabling secure boot failed with the log "Checksum failure". How to fix it?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - After enabling secure boot, the size of bootloader.bin will increase, please check whether the size of the bootloader partition is enough to store the compiled bootloader.bin. For more information, please refer to `Bootloader Size <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/bootloader.html#bootloader-size>`_。


NVS encryption failed to start and an error occurred as ``nvs: Failed to read NVS security cfg: [0x1117] (ESP_ERR_NVS_CORRUPT_KEY_PART)``. How can I solve this issue?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - Please erase flash once using the flash tool before starting NVS encryption, and then flash the firmware which can enable the NVS encryption to the SoC.


After flash encryption was enabled, a warning occurred as ``esp_image: image at 0x520000 has invalid magic byte (nothing flashed here)``. How can I solve this issue?
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - After SoC starts flash encryption, it will try to encrypt the data of all the partitions of the app type. If there is no corresponding app firmware stored in one app partition, the above log will appear. To avoid this warning, you can flash pre-compiled app firmware to the partitions of the app type when starting flash encryption.

Why is reltead data not encrypted after I enable ``CONFIG_EFUSE_VIRTUAL`` and flash encryption?
-----------------------------------------------------------------------------------------------------------

  - Currently, Virtual eFuses is only used to test the update of eFuse data. Thus, flash encryption is not enabled completely even this function is enabled.

Can I update an app firmware which enables flash encryption in a device which does not enable fash encryption through OTA?
-------------------------------------------------------------------------------------------------------------------------------------------

  - Yes, please deselect ``Check Flash Encryption enabled on app startup`` when compiling.

How can I delete keys of secure boot?
--------------------------------------------------

  - Keys of secure boot should be deleted in the firmware ``new_app.bin``. First, please assure that ``new_app.bin`` is employed with two signatures. Then, flash ``new_app.bin`` to the device. At last, when the original signatures are verified, you can delete the original keys through ``esp_ota_revoke_secure_boot_public_key()`` in ``new_app.bin``. Please note that if you use the OTA rollback scheme, please call ``esp_ota_revoke_secure_boot_public_key()`` after ``esp_ota_mark_app_valid_cancel_rollback()`` returns ``ESP_OK``. For more details, please refer to `Key Revocation <https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/security/secure-boot-v2.html?highlight=esp_ota_revoke_secure_boot_public_key#key-revocation>`_.

After I enabled secure boot or flash encryption (development mode), I cannot flash the new firmware, and an error occured as ``Failed to enter Flash download mode``. How can I solve this issue?
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - Generally, the above log indicates that your flash command is incorrect. Please use script ``idf.py`` to execute ``idf.py bootloader`` and ``idf.py app`` to compile ``bootloader.bin`` and ``app.bin``. Then execute the flash command through ``idf.py`` according to the tips after compiling. If you still cannot flash your firmware, please use ``espefuse.py -p PORT summary`` to check the eFuse of the current device and check whether the flash download mode is enabled or not.

----------------------

After I input the command ``espefuse.py read_protect_efuse BLOCK3 command`` in the terminal configured with ESP-IDF to enable the read-protection for Efuse BLOCK3, why is the data of the Efuse BLOCK3 all 0x00 when I input ``esp_efuse_read_block()`` to read the Efuse BLOCK3?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - After the Efuse BLOCK3 is read protected, it cannot be read anymore.

-----------------------------------------

How can I enable secure boot or flash encryption by pre-burning eFuse?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  By default, you can enable secure boot or flash encryption by burning firmware with secure boot or flash encryption enabled. In addition, you can also enable secure boot or flash encryption by pre-burning eFuse in the following two methods:
  - With `flash_download_tool <https://www.espressif.com/zh-hans/support/download/other-tools>`__, eFuse will be pre-burned automatically if secure boot or flash encryption is enabled.
  - You can generate the key and burn corresponding eFuse blocks with `espsecure.py <https://docs.espressif.com/projects/esptool/en/latest/esp32/espsecure/index.html>`__ and `espefuse.py <https://docs.espressif.com/projects/esptool/en/latest/esp32/espefuse/index.html>`__.

------------

After enabling Secure Boot, why can't I flash the new bootloader.bin using the `idf.py build` command?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  After enabling Secure Boot, please use the `idf.py bootloader` command to compile the new bootloader.bin. Then, flash the new bootloader.bin using the command `idf.py -p (PORT) bootloader-flash`.

------------

After enabling Secure Boot or flash encryption, how can I view the security-related information in the device?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  Please use the command `esptool.py --no-stub get_security_info` to view the security information of the device.

------------

After enabling Secure Boot or flash encryption, what should I pay attention to during OTA (Over-The-Air) updates?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - After enabling Secure Boot, you must sign the new firmware to be used for OTA updates. Otherwise, the new firmware cannot be applied to the device.
  - After enabling flash encryption, when generating a new firmware, please ensure that the flash encryption option is enabled.