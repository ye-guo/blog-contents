# Nginx配置SSL

---

title: Nginx配置SSL

date: 2024-04-16

category: 技术杂谈

tags: 

- nginx
- SSL

---

**配置如下：**

```nginx
server {
	#SSL 默认访问端口号为 443
	listen 443 ssl;
	#请填写绑定证书的域名
	server_name example.com; 
 
	# gzip config
	gzip on;
	gzip_min_length 1k;
	gzip_comp_level 9;
	gzip_types text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
	gzip_vary on;
	gzip_disable "MSIE [1-6]\.";

	root /usr/share/nginx/html;
	index index.html index.htm;
	include /etc/nginx/mime.types;
    	  
	#请填写证书文件的相对路径或绝对路径 注意容器nginx配置路径是挂载对应目录
	ssl_certificate /ssl/domain_bundle.crt; 
	#请填写私钥文件的相对路径或绝对路径
	ssl_certificate_key /ssl/domain.key; 
	ssl_session_timeout 5m;
	#请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;

	#请按照以下协议配置
	ssl_protocols TLSv1.2 TLSv1.3;
	ssl_prefer_server_ciphers on;
    // 网关支持
    location ^~ /api/ { 
		rewrite ^/api(.*)$ $1 break;
		proxy_pass http://172.17.0.1:8080/;
		add_header 'Access-Control-Allow-Origin' $http_origin; 
		add_header 'Access-Control-Allow-Credentials' 'true';
		add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS'; 
		add_header Access-Control-Allow-Headers '*'; 
		if ($request_method = 'OPTIONS') { 
		add_header 'Access-Control-Allow-Credentials' 'true';
		add_header 'Access-Control-Allow-Origin' $http_origin; 
		add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
		add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X- Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range'; 
		add_header 'Access-Control-Max-Age' 1728000; 
		add_header 'Content-Type' 'text/plain; charset=utf-8'; 
		add_header 'Content-Length' 0; return 204; 
} 
}
    location / {
		try_files $uri /index.html;
    }
    if ($host != "example.com") {  
        return 403;  # 如果请求的域名不匹配，返回 403 禁止访问
    }

}

server {
	listen 80;
	#请填写绑定证书的域名
	server_name example; 
	#把http的域名请求转成https
	return 301 https://$host$request_uri; 
}
```



