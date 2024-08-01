在使用 Python 的 `requests` 模块调用 `https://hostname` 出现 SSL 错误时，可能的原因有多个，例如证书验证失败、不受信任的证书或缺少中间证书。Postman 可以成功请求可能是因为它忽略了 SSL 错误或者使用了不同的证书验证机制。解决这个问题有几种方法：

### 1. 忽略 SSL 验证

虽然不推荐在生产环境中使用，但在开发和调试时可以通过忽略 SSL 验证来解决问题：

```python
import requests

response = requests.get('https://hostname', verify=False)
print(response.content)
```

你会看到一个警告，可以通过 `urllib3` 库来禁用它：

```python
import requests
from requests.packages.urllib3.exceptions import InsecureRequestWarning

# 禁用警告
requests.packages.urllib3.disable_warnings(InsecureRequestWarning)

response = requests.get('https://hostname', verify=False)
print(response.content)
```

### 2. 使用自定义的 CA 证书

如果服务器使用的是自签名证书或私有 CA 证书，可以下载对应的 CA 证书，并在请求中指定：

```python
import requests

# 使用自定义的 CA 证书
response = requests.get('https://hostname', verify='/path/to/ca_bundle.crt')
print(response.content)
```

### 3. 更新 `certifi` 包

`requests` 库使用 `certifi` 包来验证 SSL 证书。确保你安装了最新版本的 `certifi`：

```bash
pip install --upgrade certifi
```

### 4. 使用 `ssl` 库自定义 SSL 上下文

对于高级用户，可以使用 `ssl` 库来自定义 SSL 上下文：

```python
import requests
import ssl

# 创建一个 SSL 上下文对象
context = ssl.create_default_context()
context.check_hostname = False
context.verify_mode = ssl.CERT_NONE

response = requests.get('https://hostname', verify=context)
print(response.content)
```

### 5. 检查系统的 CA 证书

有时候，系统上的 CA 证书可能缺失或未更新，可以尝试更新系统的 CA 证书或手动安装需要的证书。

### 6. 捕获并处理 SSL 错误

如果你想捕获并处理 SSL 错误，可以使用 `requests.exceptions.SSLError`：

```python
import requests
from requests.exceptions import SSLError

try:
    response = requests.get('https://hostname')
    print(response.content)
except SSLError as e:
    print(f"SSL error occurred: {e}")
```

### 综合示例

下面是一个综合示例，展示了如何处理 SSL 错误并忽略 SSL 验证：

```python
import requests
from requests.exceptions import SSLError
from requests.packages.urllib3.exceptions import InsecureRequestWarning

# 禁用 InsecureRequestWarning
requests.packages.urllib3.disable_warnings(InsecureRequestWarning)

try:
    response = requests.get('https://hostname', verify=False)
    print(response.content)
except SSLError as e:
    print(f"SSL error occurred: {e}")
```

以上方法应该能够帮助你解决在使用 `requests` 模块时遇到的 SSL 错误问题。如果你有自签名证书或私有 CA 证书，推荐使用第二种方法。