## 1.VPS安装Debian8 更新系统 安装必要软件

```
apt update -y

apt install curl -y
```

## 2.执行官方脚本 安装v2ray

```
bash <(curl -L -s https://install.direct/go.sh)
```

## 3.配置v2ray

```
touch /etc/v2ray/config.json

cat <<EOF > /etc/v2ray/config.json
{
  "log": {
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log",
    "loglevel": "warning"
  },
  "dns": {},
  "stats": {},
  "inbounds": [
    {
      "port": 2087,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "c1fa2c00-c45c-42bd-8d28-6ee844a88a2d",
            "alterId": 6
          }
        ]
      },
      "tag": "in-0",
      "streamSettings": {
        "network": "tcp",
        "security": "none",
        "tcpSettings": {}
      }
    }
  ],
  "outbounds": [
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag": "blocked",
      "protocol": "blackhole",
      "settings": {}
    }
  ],
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "blocked"
      }
    ]
  },
  "policy": {},
  "reverse": {},
  "transport": {}
}
EOF
```

## 4.重启v2ray 载入配置文件

```
systemctl restart v2ray
```

## 5.客户端配置

手动配置 [推荐]

```
地址(address)：199.187.125.84
端口(port)：2087
用户ID(id)：c1fa2c00-c45c-42bd-8d28-6ee844a88a2d
额外ID(alterId)：6
加密方式(security)：auto
传输协议(network)：kcp
伪装类型(type)：none
```

客户端json 一键导入（不同客户端可能需修改inbounds port 如v2rayNG为10808）

```
{
  "log": {},
  "dns": {},
  "stats": {},
  "inbounds": [
    {
      "port": "1080",
      "protocol": "socks",
      "settings": {
        "auth": "noauth",
        "udp": true
      },
      "tag": "in-0"
    },
    {
      "port": "1081",
      "protocol": "http",
      "settings": {},
      "tag": "in-1"
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "199.187.125.84",
            "port": 2087,
            "users": [
              {
                "id": "c1fa2c00-c45c-42bd-8d28-6ee844a88a2d",
                "alterId": 6
              }
            ]
          }
        ]
      },
      "tag": "out-0",
      "streamSettings": {
        "network": "tcp",
        "security": "none",
        "tcpSettings": {}
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag": "blocked",
      "protocol": "blackhole",
      "settings": {}
    }
  ],
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "ip": [
          "geoip:cn"
        ],
        "inboundTag": [
          "in-0",
          "in-1"
        ],
        "outboundTag": "direct"
      }
    ]
  },
  "policy": {},
  "reverse": {},
  "transport": {}
}
```

## 6.附录

```
为减轻服务器负担 额外ID仅设置为6，如有必要可以酌情配置

配置文件参照
https://github.com/veekxt/v2ray-template/tree/master/tcp%2Bvmess%2BCN_direct

其它配置文件自行参照
https://github.com/veekxt/v2ray-template
```