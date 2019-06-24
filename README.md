## 一步一步教你安装和配置 v2ray 服务端

教程以 mKCP+无伪装 为例，其它配置复制替换下方相关代码即可。

```
其它配置文件自行参照：
https://github.com/veekxt/v2ray-template
https://github.com/KiriKira/vTemplate

https://veekxt.com/utils/v2ray_gen
https://intmainreturn0.com/v2ray-config-gen

涉及到tls的相关配置（如：ws、h2）不能仅使用下方代码配置
```

## 使用方法：按照空行分出的段落，在Xshell依次执行以下命令即可。

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
  "inbounds": [
    {
      "port": 16823,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "b831381d-6324-4d53-ad4f-8cda48b30811",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "mkcp",
        "kcpSettings": {
          "uplinkCapacity": 5,
          "downlinkCapacity": 100,
          "congestion": true,
          "header": {
            "type": "none"
          }
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
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
地址(address)：103.1.14.203
端口(port)：16823
用户ID(id)：b831381d-6324-4d53-ad4f-8cda48b30811
额外ID(alterId)：6
加密方式(security)：none
传输协议(network)：kcp
伪装类型(type)：none
```

客户端json 一键导入（不同客户端可能需修改inbounds port 如v2rayNG为10808）

```
{
  "inbounds": [
    {
      "port": 1087,
      "listen": "127.0.0.1",
      "protocol": "http",
      "settings": {}
    },
    {
      "port": 1080,
      "protocol": "socks",
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
    },
      "settings": {
        "auth": "noauth"
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "103.1.14.203",
            "port": 16823,
            "users": [
              {
                "id": "b831381d-6324-4d53-ad4f-8cda48b30811",
                "alterId": 6
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "mkcp",
        "kcpSettings": {
          "uplinkCapacity": 5,
          "downlinkCapacity": 100,
          "congestion": true,
          "header": {
            "type": "none"
          }
        }
      }
    }
  ]
}
```

## 6.附录

```
为减轻服务器负担 额外ID仅设置为6，如有必要可以酌情配置

新手推荐使用Debian系统
CentOS需要手动开放防火墙端口 或者使用以下命令直接关闭防火墙：

systemctl stop firewalld
systemctl disable firewalld

配置文件参照
https://toutyrater.github.io/advanced/mkcp.html
```


## 关联项目：

```
https://c2ray.ml

https://github.com/dylanbai8/c2ray
```


