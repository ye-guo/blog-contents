# Nginx二级域名反向代理

---

title: Nginx二级域名反向代理

date: 2024-04-15 

category: 技术杂谈

tags: 

- nginx
- 反向代理

---

**配置如下：**

```nginx
server {
    # SSL 默认访问端口号为 443
    listen 443 ssl;
    listen [::]:443 ssl;  # 添加IPv6监听
    # 请填写绑定证书的域名
    server_name domain.com;
    # 请填写证书文件的相对路径或绝对路径
    ssl_certificate  /etc/nginx/ssl/fullchain.crt;
    # 请填写私钥文件的相对路径或绝对路径
    ssl_certificate_key /etc/nginx/ssl/private.key;
    ssl_session_timeout 5m;
    # 请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    # 请按照以下协议配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;
    ssl_stapling on;
    ssl_stapling_verify on;
    location / {
        # 网站主页路径。此路径仅供参考，具体请您按照实际目录操作。
        # 例如，您的网站主页在 Nginx 服务器的 /etc/www 目录下，则请修改 root 后面的 html 为 /etc/www。
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://127.0.0.1:port;
    }
}

server {
    listen 80;
    listen [::]:80;  # 添加IPv6监听
    # 请填写绑定证书的域名
    server_name domain.com;
    # 把http的域名请求转成https
    return 301 https://$host$request_uri;
}
```

