# 博流WIFI驱动开发资料

## RT-Thread WIFI驱动接口

[RT-Thread WIFI驱动开发文档](https://www.rt-thread.org/document/site/#/rt-thread-version/rt-thread-standard/programming-manual/device/wlan/wlan)

### w60x WiFi驱动开发示例

[drv_wifi.h](https://github.com/RT-Thread/rt-thread/blob/master/bsp/w60x/drivers/drv_wifi.h)

[drv_wifi.c](https://github.com/RT-Thread/rt-thread/blob/master/bsp/w60x/drivers/drv_wifi.c)

### 要实现的接口

``` C++
struct rt_wlan_dev_ops
{
    rt_err_t (*wlan_init)(struct rt_wlan_device *wlan); // WIFI初始化
    rt_err_t (*wlan_mode)(struct rt_wlan_device *wlan, rt_wlan_mode_t mode); // WIFI连接模式设置 RT_WLAN_NONE RT_WLAN_STATION RT_WLAN_AP RT_WLAN_MODE_MAX
    rt_err_t (*wlan_scan)(struct rt_wlan_device *wlan, struct rt_scan_info *scan_info); // WIFI 扫描
    rt_err_t (*wlan_join)(struct rt_wlan_device *wlan, struct rt_sta_info *sta_info); // WIFI 加入
    rt_err_t (*wlan_softap)(struct rt_wlan_device *wlan, struct rt_ap_info *ap_info); // 
    rt_err_t (*wlan_disconnect)(struct rt_wlan_device *wlan); //WIFI 断开
    rt_err_t (*wlan_ap_stop)(struct rt_wlan_device *wlan); //关闭AP
    rt_err_t (*wlan_ap_deauth)(struct rt_wlan_device *wlan, rt_uint8_t mac[]);
    rt_err_t (*wlan_scan_stop)(struct rt_wlan_device *wlan); //停止扫描
    int (*wlan_get_rssi)(struct rt_wlan_device *wlan); //获得信号强度。信号强度为负值，值越大信号越强（-1强度高 > -10强度低）
    rt_err_t (*wlan_set_powersave)(struct rt_wlan_device *wlan, int level); //设置功耗等级
    int (*wlan_get_powersave)(struct rt_wlan_device *wlan); //获取当前功耗等级
    rt_err_t (*wlan_cfg_promisc)(struct rt_wlan_device *wlan, rt_bool_t start); // 配置混杂模式 
    rt_err_t (*wlan_cfg_filter)(struct rt_wlan_device *wlan, struct rt_wlan_filter *filter); 
    rt_err_t (*wlan_cfg_mgnt_filter)(struct rt_wlan_device *wlan, rt_bool_t start);
    rt_err_t (*wlan_set_channel)(struct rt_wlan_device *wlan, int channel); // 设置通道
    int (*wlan_get_channel)(struct rt_wlan_device *wlan); // 获取通道
    rt_err_t (*wlan_set_country)(struct rt_wlan_device *wlan, rt_country_code_t country_code); //设置. 国家码 国家码概念 http://www.wlan-wwan.com/2020/02/08/shenmeshiwlanguojiama/
    rt_country_code_t (*wlan_get_country)(struct rt_wlan_device *wlan); //获取国家码
    rt_err_t (*wlan_set_mac)(struct rt_wlan_device *wlan, rt_uint8_t mac[]); // 设置mac
    rt_err_t (*wlan_get_mac)(struct rt_wlan_device *wlan, rt_uint8_t mac[]); // 获取mac
    int (*wlan_recv)(struct rt_wlan_device *wlan, void *buff, int len); // 获取内容
    int (*wlan_send)(struct rt_wlan_device *wlan, void *buff, int len); // 发送内容
    int (*wlan_send_raw_frame)(struct rt_wlan_device *wlan, void *buff, int len); 
    int (*wlan_get_fast_info)(void *data);
    rt_err_t (*wlan_fast_connect)(void *data,rt_int32_t len);
};
```

### 其他参考文献

[RT-Thread学习笔记【WLAN】](https://ww.dandelioncloud.cn/article/details/1586895571359354881)

[IOT-OS之RT-Thread（十六）--- WLAN管理框架 + AP6181(BCM43362) WiFi模块](https://blog.csdn.net/m0_37621078/article/details/105264345)

## 博流芯片 WIFI SDK

### BL_MCU_SDK

目前BL_MCU_SDK仅支持BL616芯片，代码见[https://github.com/bouffalolab/bouffalo_sdk/tree/master/examples/wifi](https://github.com/bouffalolab/bouffalo_sdk/tree/master/examples/wifi)

M1s_BL808_SDK 同样支持WIFI相关驱动，代码见[https://github.com/sipeed/M1s_BL808_example/tree/main/c906_app/camera_streaming_through_wifi](https://github.com/sipeed/M1s_BL808_example/tree/main/c906_app/camera_streaming_through_wifi)