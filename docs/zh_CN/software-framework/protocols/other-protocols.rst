其他协议
===============

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

ESP32 如何优化通信延时？
------------------------------------

  - 建议关闭 Wi-Fi 休眠功能，调用 API ``esp_wifi_set_ps(WIFI_PS_NONE)``。
  - 建议在 menucongfig 关掉 ``AMPDU`` 功能。

--------------

ESP8285 是否⽀持 CCS (Cisco Compatible eXtensions)？
-----------------------------------------------------------------

  ESP8285 不支持 CCS (Cisco Compatible eXtensions)。

--------------

ESP32 是否支持 LoRa (Long Range Radio) 通信？
------------------------------------------------------------

  ESP32 自身并不支持 LoRa 通信，芯片没有集成 LoRa 协议栈与对应的射频部分。ESP32 可以外接集成 LoRa 协议的芯⽚，作为主控 MCU 连接 LoRa 芯片，可以实现 Wi-Fi 与 LoRa 设备的通信。

--------------

ESP32-S2 在调用 ``esp_netif_t* wifiAP  = esp_netif_create_default_wifi_ap()`` 后通过 ``esp_netif_destroy(wifiAP)`` 注销会产生 12 字节的内存泄露，什么原因？
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  - 需要在 ``esp_netif_destroy(wifiAP)`` 前额外调用 ``esp_wifi_clear_default_wifi_driver_and_handlers(wifiAP)``，这样才是正确的注销流程，此时可发现内存泄露的情况已消失。
  - 也可以直接调用 ``esp_netif_destroy_default_wifi(wifiAP)``，该接口在 ESP-IDF v4.4 版本以上支持。

--------------

如何实现证书自动下载功能 ?
---------------------------------------------------------------------------------------------------------------------------------------------------------

  :CHIP\: ESP32:

  具体操作详情参考 `aws 下面证书自动下载功能 <https://docs.aws.amazon.com/zh_cn/iot/latest/developerguide/auto-register-device-cert.html>`_ 。

-----------------------------

ESP-IDF 里如何根据错误码来获取更多的调试信息？
--------------------------------------------------------------------------------------------------------------------------------

  - ESP-IDF 3.x 版本下的错误码 (errno) 列表直接存在于 IDF 中，点击 `errno.h <https://github.com/espressif/esp-idf/blob/release/v3.3/components/newlib/include/sys/errno.h>`_ 可以进行查询。
  - ESP-IDF 4.x 版本的 ``errno.h`` 位于编译器工具链下，比如，对于 esp-2020r3 而言，errno.h 的路径为 ``/root/.espressif/tools/xtensa-esp32-elf/esp-2020r3-8.4.0/xtensa-esp32-elf/xtensa-esp32-elf/include/sys/errno.h``。

----------------

ESP8266_RTOS_SDK 是否支持 TR-069 协议？
-----------------------------------------------------------------------------------------------------------

  不支持，ESP8266_RTOS_SDK 本身并不提供对 TR-069 协议的原生支持，但开发者可以根据自己的需求自行实现 TR-069 协议栈，并将其集成到 ESP8266_RTOS_SDK 中。也可以使用现有的第三方 TR-069 协议栈进行集成。总之，ESP8266_RTOS_SDK 可以支持 TR-069 协议，但需要进行自行集成实现。

----------------

ESP32 支持 SAVI 吗？
-----------------------------------------------------------------------------------------------------------

  不支持，SAVI (Source Address Validation Improvements) 是通过监听控制类报文（如 ND、DHCPv6），即 CPS (Control Packet Snooping)，在接入设备上（AP 或交换机）为终端建立基于 IPv6 源地址、源 MAC 地址及接入设备端口的绑定关系，进而对通过指定端口的 IP 报文进行源地址校验。只有报文源地址与绑定表项匹配时才可以转发，保证网络上数据报文源地址真实性。这种一般针对于交换机或者企业级 AP 路由器的，是策略协议。目前 ESP32 支持 IPv6 链路本地地址、全球地址通信。

--------------------------------

以太网 和 Wi-Fi 并存时，优先使用以太网传输数据吗？
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :CHIP\: ESP32 :

  - 先调用 `esp_netif_get_route_prio <https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/api-reference/network/esp_netif.html#_CPPv424esp_netif_get_route_prioP11esp_netif_t>`_ 查看以太网和 Wi-Fi 的优先级，如果 Wi-Fi 的优先级高于以太网， 可以通过修改 ``esp_netif_t`` 结构体里的 ``route_prio`` 来改变优先级。
  
