触摸传感器
============

:link_to_translation:`en:[English]`

.. raw:: html

   <style>
   body {counter-reset: h2}
     h2 {counter-reset: h3}
     h2:before {counter-increment: h2; content: counter(h2) ". "}
     h3:before {counter-increment: h3; content: counter(h2) "." counter(h3) ". "}
     h2.nocount:before, h3.nocount:before, { content: ""; counter-increment: none }
   </style>

--------------

使⽤ ESP32 做触摸相关应⽤时，哪⾥有相关资料可参考？
------------------------------------------------------------------

  请参考推荐的 `软硬件设计 <https://github.com/espressif/esp-iot-solution/tree/release/v1.1/examples/touch_pad_evb>`_。

--------------

ESP32-S2 Touch Sensor 的防水功能是在有水时屏蔽 Touch 还是有水时仍然能识别 Touch 事件？
----------------------------------------------------------------------------------------------------------

  ESP32-S2 Touch Sensor 的防水功能可以在有少量水珠场景下正常工作。在有大面积积水（Touch 触点区域被完全覆盖）的情况下，Touch 会暂时锁定，直到积水被清除后 Touch 才能恢复正常工作。

--------------

ESP32-S2 Touch Sensor 的防水流功能在屏蔽有水流的 Touchpad 时，是否能够保持未沾水的 Pad 仍能使用？
----------------------------------------------------------------------------------------------------------------------

  可以，可通过软件选择具体屏蔽的通道。

--------------

是否有推荐的可以用于 Touch Sensor 测试、稳定触发 Touch Sensor 并且参数与人手触摸时参数接近的材料？
----------------------------------------------------------------------------------------------------------------------------------------------------------

  对一致性要求较高的实验，可使用手机电容笔来替代人手进行测试。

--------------

Touch Sensor 的管脚能否重映射？
----------------------------------------------------------------

  不能，因为 Touch Sensor 属于模拟信号处理。

--------------

在覆盖亚克力板后，Touch Sensor 检测阈值是否需要重新设置？
-----------------------------------------------------------------------------------------------

  需要重新设置一个阈值。

--------------

Touch Sensor 能否检测是否有亚克力板覆盖，以便在添加或移除亚克力板时，自动切换预设定的检测阈值？
----------------------------------------------------------------------------------------------------

  暂时不能自动适应覆盖层物理参数变化所带来的影响。

----------------

ESP32 触摸屏有哪些参考驱动？
--------------------------------------------------------------

  - 代码请参考 `touch_panel_code <https://github.com/espressif/esp-iot-solution/tree/master/components/display/touch_panel>`_。
  - 文档请参考 `touch_panel_doc <https://docs.espressif.com/projects/espressif-esp-iot-solution/zh_CN/latest/input_device/touch_panel.html>`_。
