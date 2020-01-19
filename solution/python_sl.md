# Python解决方案

----

## No module named '_ssl'

### 更新配置
  * 更新Python目录下的（`/usr/local/Python-3.7.6/`）`Modules/Setup.dist`。

```
// 将以下注释放开（大概在 209 行）

SSL=/usr/local/ssl
_ssl _ssl.c \
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
-L$(SSL)/lib -lssl -lcrypto
```

### 重新编译
  * 返回Python目录。

```
./configure

make && make install
```

### 验证
```
python3

import ssl;
```