# let's Encrypt

---

title: let's Encrypt

date: 2024-07-22

category: 技术杂谈

tags: 

- certificate
- nginx

---

**我的环境是Debian11、Nginx，选择自己的环境**

https://certbot.eff.org/instructions

## 一、安装snapd

可以在官方找到各个操作系统安装教程

https://snapcraft.io/docs/installing-snapd

我这里使用的Debian

```bash
sudo apt update
sudo apt install snapd
```

测试是否安装成功

```bash
sudo snap install hello-world
# hello-world 6.3 from Canonical✓ installed
$ hello-world
# Hello World!
```

## 二、安装Certbot

```bash
sudo snap install --classic certbot
```

确保certbot命令可以运行

```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

## 三、直接生成证书

```bash
sudo certbot certonly --standalone -d example.com -d www.example.com
```

## 四、DNS验证方式

### 1、配置snap插件信任

```bash
sudo snap set certbot trust-plugin-with-root=ok
```

### 2、安装DNS插件

Certbot 提供了一系列 DNS 插件，适用于不同的 DNS 提供商。

* Cloudflare 插件

  ```bash
  sudo snap install certbot-dns-cloudflare
  ```

* AWS 插件：

  ```bash
  sudo snap install certbot-dns-route53
  ```

* DigitalOcean 插件：

  ```bash
  sudo snap install certbot-dns-route53
  ```

### 3、配置 DNS API 凭证

我使用的是Cloudflare的DNS

```bash
mkdir -p ~/.secrets/certbot
echo "dns_cloudflare_email = your-email@example.com" >> ~/.secrets/certbot/cloudflare.ini
echo "dns_cloudflare_api_key = your-api-key" >> ~/.secrets/certbot/cloudflare.ini
chmod 600 ~/.secrets/certbot/cloudflare.ini
```

### 4、生成泛域名证书

```bash
sudo certbot certonly --dns-cloudflare --dns-cloudflare-credentials ~/.secrets/certbot/cloudflare.ini -d "*.example.com" -d "example.com"
```

这里包含了根域名，这种方式生成后需要手动配置

成功！

![image-20240722120707052](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/image-20240722120707052.png)

生成的证书和密钥一般在/etc/letsencrypt/live/example.com目录下

## 五、测试自动续约

let's encrypt 的证书一般只有三个月时效

```bash
sudo certbot renew --dry-run
```

![image-20240722120940851](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/image-20240722120940851.png)

配置好证书后，登录网站查看证书

![image-20240722145532497](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/image-20240722145532497.png)

