# Nginx

[toc]

重新加载配置文件
nginx -s reload

## 1.静态HTTP服务器
```
server{
    Listen 80;
    Location {
        Root  /usr/share/nginx/html
    }
}
```

## 2.反向代理
```
server {
   	listen 80;
    location ~ / {
        proxy_pass http://admin_api.net; # 应用服务器HTTP地址
    }
}
```

## 3.负载均衡
```
upstream myapp {
	ip_hash; # 根据客户端IP地址Hash值将请求分配给固定的一个服务器处理
    server 192.168.0.1:8084;  // 应用服务器1
    server 192.168.0.2:8084;  // 应用服务器2
}
server {
    listen 80;
    location ~ / {
        proxy_pass http://myapp; # 应用服务器HTTP地址
    }
}
```

> 以上配置会将请求轮询分配到应用服务器，也就是一个客户端的多次请求，有可能会由多台不同的服务器处理。可以通过ip-hash的方式，根据客户端ip地址的hash值将请求分配给固定的某一个服务器处理。

> 另外，服务器的硬件配置可能有好有差，想把大部分请求分配给好的服务器，把少量请求分配给差的服务器，可以通过weight来控制。


```
upstream myapp {
    server192.168.20.1:8080weight=3; # 该服务器处理3/4请求
    server192.168.20.2:8080; # weight默认为1，该服务器处理1/4请求
}
```

## 4.虚拟主机
> 有的网站访问量大，需要负载均衡。然而并不是所有网站都如此出色，有的网站，由于访问量太小，需要节省成本，将多个网站部署在同一台服务器上。

> 例如将www.aaa.com和www.bbb.com两个网站部署在同一台服务器上，两个域名解析到同一个IP地址，但是用户通过两个域名却可以打开两个完全不同的网站，互相不影响，就像访问两个服务器一样，所以叫两个虚拟主机。

```
server {
    listen80default_server;
    server_name _;
    return444; # 过滤其他域名的请求，返回444状态码
}
server {
    listen80;
    server_name www.aaa.com; # www.aaa.com域名
    location / {
        proxy_pass http://localhost:8080; # 对应端口号8080
    }
}
server {
    listen80;
    server_name www.bbb.com; # www.bbb.com域名
    location / {
        proxy_pass http://localhost:8081; # 对应端口号8081
    }
}

```

## 5.FastCGI
> Nginx本身不支持PHP等语言，但是它可以通过FastCGI来将请求扔给某些语言或框架处理（例如PHP、Python、Perl）。

```
server {
    listen80;
    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME /PHP文件路径$fastcgi_script_name; # PHP文件路径
        fastcgi_pass127.0.0.1:9000; # PHP-FPM地址和端口号
        # 另一种方式：fastcgi_pass unix:/var/run/php5-fpm.sock;
    }
}
```

> 配置中将.php结尾的请求通过FashCGI交给PHP-FPM处理，PHP-FPM是PHP的一个FastCGI管理器。