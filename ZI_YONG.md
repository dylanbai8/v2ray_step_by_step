## 自用配置备记

```
bash <(curl -L -s https://install.direct/go.sh)

touch /etc/v2ray/config.json

cat <<EOF > /etc/v2ray/config.json
{
  "inbounds": [
    {
      "port": 7017,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "78266cb5-8860-4d12-9095-296f784d4891",
            "alterId": 72
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
            "type": "utp"
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