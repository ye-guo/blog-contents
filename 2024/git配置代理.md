# git配置代理

---

title:  git配置代理

date: 2024-08-4 17:09

category: 技术杂谈

tags:

- git
- proxy

---

解决vscode push经常失败问题

### 1.设置代理

* socks 代理

```bash
git config --global http.proxy socks5://127.0.0.1:64435  
git config --global https.proxy socks5://127.0.0.1:64435
```

-  HTTP/HTTPS 代理

```bash
git config --global http.proxy http://127.0.0.1:64436  
git config --global https.proxy https://127.0.0.1:64436
```

### 2.验证设置

验证你的代理设置是否正确：

```bash
git config --global --get http.proxy  
git config --global --get https.proxy
```

### 3.列出所有代理设置

```bash
git config --global --get-all http.proxy  
git config --global --get-all https.proxy
```

这将列出所有为 `http.proxy` 和 `https.proxy` 设置的值。

### 4.清理代理

```bash
git config --global --unset-all http.proxy  
git config --global --unset-all https.proxy
```

