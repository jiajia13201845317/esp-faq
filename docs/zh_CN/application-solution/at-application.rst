AT
==

:link_to_translation:`en:[English]`

.. raw:: html

   <style>
   body {counter-reset: h2}
     h2 {counter-reset: h3}
     h2:before {counter-increment: h2; content: counter(h2) ". "}
     h3:before {counter-increment: h3; content: counter(h2) "." counter(h3) ". "}
     h2.nocount:before, h3.nocount:before, { content: ""; counter-increment: none }
   </style>

有关 ESP-AT 的常见问题请见 `ESP-AT 用户指南 <https://docs.espressif.com/projects/esp-at/zh_CN/latest/faq.html>`_.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

使用 AT  `网页编译 <https://docs.espressif.com/projects/esp-at/zh_CN/latest/esp32c2/Compile_and_Develop/How_to_build_project_with_web_page.html>`_ 工程，如何修改 sdkconfig enable BLE 功能 ?
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :CHIP\: ESP32-C2 :

  - 在 sdkconfig_default 中需要修改的配置项如下：

  .. code-block:: c

      # NimBLE Options
       CONFIG_BT_NIMBLE_GAP_DEVICE_NAME_MAX_LEN=32
       CONFIG_BT_NIMBLE_ATT_PREFERRED_MTU=203

      # AT
       CONFIG_AT_BLE_COMMAND_SUPPORT=y

