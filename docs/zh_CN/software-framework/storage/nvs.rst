非易失性存储 (NVS)
====================

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

若每分钟保存或者更新数据到 flash 中，ESP32 设备的 NVS 能否满足该需求？
-----------------------------------------------------------------------------------

  根据 `NVS 说明 <https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/api-reference/storage/nvs_flash.html>`_，NVS 库在其操作中主要使用两个实体：页面和条目。逻辑页面对应 flash 的一个物理扇区。假设 flash 扇区大小为 4096 字节，每个页面可容纳 126 个条目（每个条目大小为 32 字节），页面的其余部分用于页头部（32 字节）和条目状态位图（32 字节）。每个扇区的典型 flash 寿命为 100 k 个擦除周期。假设期待设备的运行时间为 10 年，每分钟写入 flash 的数据大小为 4 字节，并且不使用 flash 加密，计算 flash 写操作的次数为：60×24×365×10=5256000。这样，在 NVS 中会导致不超过 42 k 个擦除周期 (5256000/126)，而 42 k < 100 k，因此，即使在没有多扇区影响的情况下也可以支持。在实际使用中，分配给 NVS 的大小一般为多个扇区，NVS 会在多扇区之间分配擦除周期，那么每个扇区的擦除周期的次数必然小于 42 k。

  因此，NVS 可以满足该擦写需求。

--------------

NVS 是否具有磨损均衡？
----------------------------

  是的，NVS（Non-Volatile Storage）具有磨损均衡（wear leveling）功能。在使用 flash 存储数据时，由于 flash 的写入和擦除次数有限，会导致一些存储块的寿命比其他块更短，从而影响整个存储器的寿命。为了解决这个问题，NVS 使用的不是 ESP-IDF 中的 wear_levelling 组件，而是在其内部实现的一种擦写平衡机制，将数据均匀地分布在 flash 的各个存储块中，使得每个存储块的使用次数尽可能相等，从而延长整个 flash 的寿命。

--------------

NVS 扇区是否会因写入时意外断电而损坏？
------------------------------------------------

  NVS 设计之初就具有抵抗意外断电的能力，因此不会损坏。

--------------

配置好的 Wi-Fi SSID 和 PASSWORD 在 ESP 系列开发板上重新上电后是否会消失，需要重新输入吗？
------------------------------------------------------------------------------------------------------------------------------------------------------------

  - 默认会存在 NVS 里，不会因为掉电而消失，您也可以通过 ``esp_wifi_set_storage()`` 设置，此时分为两种情况：

    - 如果想要实现掉电保存 Wi-Fi SSID 和 PSAAWORD，可通过调用 ``esp_wifi_set_storage(WIFI_STORAGE_FLASH)`` 将 Wi-Fi 信息存储在 flash 内。
    - 如果想要实现掉电不保存 Wi-Fi SSID 和 PASSWORD 的操作，可通过调用 ``esp_wifi_set_storage(WIFI_STORAGE_RAM)`` 将 Wi-Fi 信息存储在 RAM 内。

-----------------

如何实现存储的用户数据支持掉电保存，不被 OTA 擦除，且支持重写或改写？
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - 根据需求，需要使用非易失性存储，存储数据的区域只有 eFuse 或 flash。考虑到需要修改，只能使用 flash。推荐使用 NVS 或 MFG 机制。请参考：
 
    - `MFG 量产程序 <https://docs.espressif.com/projects/esp-idf/zh_CN/release-v5.0/esp32/api-reference/storage/mass_mfg.html#id1>`_ 
    - `NVS 分区生成程序 <https://docs.espressif.com/projects/esp-idf/zh_CN/release-v5.0/esp32/api-reference/storage/nvs_partition_gen.html#nvs>`_ 

