---
title: Debian-Ubuntu-安装-ssr
date: 2020-10-08 20:15:02
description: 如何使用Debian安装ssr
tags:
- Debian
- Ubuntu
- ssr
---
# 完整版

```bash
sudo apt-get install build-essential autoconf libtool libssl-dev gawk debhelper dh-systemd init-system-helpers pkg-config asciidoc xmlto apg libpcre3-dev zlib1g-dev git make libsodium-dev -y
git clone https://github.com/shadowsocksrr/shadowsocksr-libev.git
cd shadowsocksr-libev.git
./configure
修改 src/Makefile.in 和 src/Makefile.am 文件，删除中间的 -Werror
make
配置config.json文件
./src/ss-local -c config.json
```

# 详细版

## 安装编译环境

```bash
sudo apt-get install build-essential autoconf libtool libssl-dev gawk debhelper dh-systemd init-system-helpers pkg-config asciidoc xmlto apg libpcre3-dev zlib1g-dev git make  -y
```

## 下载ssr & 配置

```bash
git clone https://github.com/shadowsocksrr/shadowsocksr-libev.git
cd shadowsocksr-libev.git
./configure
```

(虽然写着是ss，但实际用起来是ssr)

如果这时直接make的话，会报错

```bash
acl.c:74:90: error: '%s' directive writing up to 63 bytes into a region of size between 50 and 176 [-Werror=format-overflow=]
     "ip6tables -N %s; ip6tables -F %s; ip6tables -A OUTPUT -p tcp --tcp-flags RST RST -j %s";
                                                                                          ^~
acl.c:158:68:
         sprintf(cli, ip6tables_init_chain, chain_name, chain_name, chain_name);
                                                                    ~~~~~~~~~~
acl.c:158:9: note: 'sprintf' output between 81 and 270 bytes into a destination of size 256
         sprintf(cli, ip6tables_init_chain, chain_name, chain_name, chain_name);
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:67:87: error: '%s' directive writing up to 63 bytes into a region of size between 53 and 179 [-Werror=format-overflow=]
     "iptables -N %s; iptables -F %s; iptables -A OUTPUT -p tcp --tcp-flags RST RST -j %s";
                                                                                       ^~
acl.c:160:67:
         sprintf(cli, iptables_init_chain, chain_name, chain_name, chain_name);
                                                                   ~~~~~~~~~~
acl.c:160:9: note: 'sprintf' output between 78 and 267 bytes into a destination of size 256
         sprintf(cli, iptables_init_chain, chain_name, chain_name, chain_name);
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:92:5: error: ';      firewall-cmd --direct...' directive writing 88 bytes into a region of size between 33 and 159 [-Werror=format-overflow=]
     "firewall-cmd --direct --add-chain ipv6 filter %s; \
     ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      firewall-cmd --direct --passthrough ipv6 -F %s; \
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      firewall-cmd --direct --passthrough ipv6 -A OUTPUT -p tcp --tcp-flags RST RST -j %s";
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:153:9: note: 'sprintf' output between 186 and 375 bytes into a destination of size 256
         sprintf(cli, firewalld6_init_chain, chain_name, chain_name, chain_name);
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:81:5: error: ';      firewall-cmd --direct...' directive writing 88 bytes into a region of size between 33 and 159 [-Werror=format-overflow=]
     "firewall-cmd --direct --add-chain ipv4 filter %s; \
     ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      firewall-cmd --direct --passthrough ipv4 -F %s; \
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      firewall-cmd --direct --passthrough ipv4 -A OUTPUT -p tcp --tcp-flags RST RST -j %s";
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:155:9: note: 'sprintf' output between 186 and 375 bytes into a destination of size 256
         sprintf(cli, firewalld_init_chain, chain_name, chain_name, chain_name);
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c: In function 'free_block_list':
acl.c:96:5: error: '%s' directive writing up to 63 bytes into a region of size between 61 and 124 [-Werror=format-overflow=]
     "firewall-cmd --direct --passthrough ipv6 -D OUTPUT -p tcp --tcp-flags RST RST -j %s; \
     ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      firewall-cmd --direct --passthrough ipv6 -F %s; \
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      firewall-cmd --direct --remove-chain ipv6 filter %s";
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:184:59:
         sprintf(cli, firewalld6_remove_chain, chain_name, chain_name, chain_name);
                                                           ~~~~~~~~~~
acl.c:184:9: note: 'sprintf' output between 189 and 378 bytes into a destination of size 256
         sprintf(cli, firewalld6_remove_chain, chain_name, chain_name, chain_name);
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:85:5: error: '%s' directive writing up to 63 bytes into a region of size between 61 and 124 [-Werror=format-overflow=]
     "firewall-cmd --direct --passthrough ipv4 -D OUTPUT -p tcp --tcp-flags RST RST -j %s; \
     ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      firewall-cmd --direct --passthrough ipv4 -F %s; \
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      firewall-cmd --direct --remove-chain ipv4 filter %s";
      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:186:58:
         sprintf(cli, firewalld_remove_chain, chain_name, chain_name, chain_name);
                                                          ~~~~~~~~~~
acl.c:186:9: note: 'sprintf' output between 189 and 378 bytes into a destination of size 256
         sprintf(cli, firewalld_remove_chain, chain_name, chain_name, chain_name);
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:76:90: error: '%s' directive writing up to 63 bytes into a region of size between 50 and 176 [-Werror=format-overflow=]
     "ip6tables -D OUTPUT -p tcp --tcp-flags RST RST -j %s; ip6tables -F %s; ip6tables -X %s";
                                                                                          ^~
acl.c:179:70:
         sprintf(cli, ip6tables_remove_chain, chain_name, chain_name, chain_name);
                                                                      ~~~~~~~~~~
acl.c:179:9: note: 'sprintf' output between 81 and 270 bytes into a destination of size 256
         sprintf(cli, ip6tables_remove_chain, chain_name, chain_name, chain_name);
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
acl.c:69:87: error: '%s' directive writing up to 63 bytes into a region of size between 53 and 179 [-Werror=format-overflow=]
     "iptables -D OUTPUT -p tcp --tcp-flags RST RST -j %s; iptables -F %s; iptables -X %s";
                                                                                       ^~
acl.c:181:69:
         sprintf(cli, iptables_remove_chain, chain_name, chain_name, chain_name);
                                                                     ~~~~~~~~~~
acl.c:181:9: note: 'sprintf' output between 78 and 267 bytes into a destination of size 256
         sprintf(cli, iptables_remove_chain, chain_name, chain_name, chain_name);
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cc1: all warnings being treated as errors
make[2]: *** [Makefile:778: libshadowsocks_libev_la-acl.lo] Error 1
make[2]: Leaving directory '/home/ubuntu/application/shadowsocksr-libev/src'
make[1]: *** [Makefile:478: all-recursive] Error 1
make[1]: Leaving directory '/home/ubuntu/application/shadowsocksr-libev'
make: *** [Makefile:387: all] Error 2
```

## 修改配置

所以，[需要修改](https://github.com/shadowsocksrr/shadowsocksr-libev/issues/40#issuecomment-413930246)

```bash
修改 src/Makefile.in 和 src/Makefile.am 文件，删除中间的 -Werror
```

## 编译

```bash
make
```

## 配置config.json文件

```bash
{
    "server": "1.1.1.1",
    "server_ipv6": "::",
    "server_port": 7324,
    "local_address": "127.0.0.1",
    "local_port": 1080,

    "password": "abcdefg",
    "method": "none",
    "protocol": "auth_chain_a",
    "protocol_param": "",
    "obfs": "tls1.2_ticket_auth",
    "obfs_param": "",
    "speed_limit_per_con": 0,
    "speed_limit_per_user": 0,

    "additional_ports" : {},
    "additional_ports_only" : false,
    "timeout": 120,
    "udp_timeout": 60,
    "dns_ipv6": false,
    "connect_verbose_info": 0,
    "redirect": "",
    "fast_open": false
}
```

把server改成你ssr的ip，server_port改成你ssr的端口，password改成你ssr的密码，method，protocol，obfs 分别安装机场给你的填

## 运行

```
./src/ss-local -c config.json
```

