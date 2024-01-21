好的，我可以给您一些具体的命令，但是您需要根据您的实际情况进行修改。以下是一个示例：

- 安装bind软件包：

```bash
sudo apt update
sudo apt install bind9
```

- 编辑/etc/named.conf文件：

```bash
sudo nano /etc/named.conf
```

在文件中添加以下内容：

```conf
options {
    directory "/var/cache/bind";
    listen-on port 53 { 127.0.0.1; 192.168.1.100; }; # 修改为您的dns服务器的ip地址
    allow-query { localhost; 192.168.1.0/24; }; # 修改为您的网络范围
    forwarders {
        8.8.8.8; # 修改为您想要转发的dns服务器的ip地址
    };
};

include "/etc/named.conf.local";
```

- 编辑/etc/named.conf.local文件：

```bash
sudo nano /etc/named.conf.local
```

在文件中添加以下内容：

```conf
zone "example.com" { # 修改为您要解析的域名
    type master;
    file "/etc/bind/db.example.com"; # 修改为您的数据文件的位置
};

zone "1.168.192.in-addr.arpa" { # 修改为您的反向域名
    type master;
    file "/etc/bind/db.192.168.1"; # 修改为您的数据文件的位置
};
```

- 编辑/etc/bind/db.example.com文件：

```bash
sudo cp /etc/bind/db.local /etc/bind/db.example.com
sudo nano /etc/bind/db.example.com
```

在文件中修改以下内容：

```conf
;
; BIND data file for example.com
;
$TTL    604800
@       IN      SOA     ns1.example.com. root.example.com. ( # 修改为您的SOA记录
                2024012101         ; Serial
                604800         ; Refresh
                86400         ; Retry
                2419200         ; Expire
                604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.example.com. # 修改为您的NS记录
@       IN      A       192.168.1.100 # 修改为您的A记录
ns1     IN      A       192.168.1.100 # 修改为您的A记录
www     IN      A       192.168.1.100 # 修改为您的A记录
```

- 编辑/etc/bind/db.192.168.1文件：

```bash
sudo cp /etc/bind/db.127 /etc/bind/db.192.168.1
sudo nano /etc/bind/db.192.168.1
```

在文件中修改以下内容：

```conf
;
; BIND reverse data file for 192.168.1.0/24
;
$TTL    604800
@       IN      SOA     ns1.example.com. root.example.com. ( # 修改为您的SOA记录
                2024012101         ; Serial
                604800         ; Refresh
                86400         ; Retry
                2419200         ; Expire
                604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.example.com. # 修改为您的NS记录
100     IN      PTR     example.com. # 修改为您的PTR记录
100     IN      PTR     ns1.example.com. # 修改为您的PTR记录
100     IN      PTR     www.example.com. # 修改为您的PTR记录
```

- 重启named服务：

```bash
sudo systemctl restart bind9
```

- 在客户端配置dns服务器的地址：

```bash
sudo nano /etc/resolv.conf
```

在文件中添加以下内容：

```conf
nameserver 192.168.1.100 # 修改为您的dns服务器的ip地址
```

- 使用nslookup或dig命令测试域名解析是否正常：

```bash
nslookup example.com
dig example.com
```

以上就是在debian 12上搭建dns服务器的具体命令。请注意，这些命令只是一个参考，您需要根据您的实际情况进行调整。如果您遇到任何问题，您可以查看我之前提供的链接，或者在网上搜索更多的教程和解决方案。祝您成功！😊