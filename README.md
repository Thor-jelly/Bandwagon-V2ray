# 暂时改换机场不更新
[如有需求可以看我暂时用的机场](https://github.com/Thor-jelly/proxy)

# Bandwagon-V2ray
Bandwagon-V2ray记录

# 搬瓦工vps v2ray

> [白话文教程](https://toutyrater.github.io/)  
> [官方教程](https://www.v2ray.com/)  
> [优惠码BWH26FXH3HIQ（优惠力度 6.25%），如果要最新请点击进入当前网站查看](https://www.bandwagonhost.net/coupon)  
> [选套餐参考-逗比根据地写的文章,最下面有更换IP政策](https://doub.io/vpszy-21/)  


# 注意

- **购买VPS，请不要挂代理，避免被IDC判断为 欺诈，从而付款了却没给你开通VPS**
- **搬瓦工的所有套餐，流量计算方式均为 双向计算。**

# [我使用的优惠码](https://www.bandwagonhost.net/coupon) 

如果需要最新优惠码请看上面链接(我也是在文章中看到的)。  

# [下载安装](https://www.v2ray.com/chapter_00/install.html)

下载 V2Ray  
预编译的压缩包可以在如下几个站点找到：  

Github Release: github.com/v2ray/v2ray-core  
Github 分流: v2ray.com/download  
Homebrew: github.com/v2ray/homebrew-v2ray  
Arch Linux: packages/community/x86_64/v2ray/  
Snapcraft: snapcraft.io/v2ray-core  
压缩包均为 zip 格式，找到对应平台的压缩包，下载解压即可使用。  

# [Linux安装脚本](https://www.v2ray.com/chapter_00/install.html)

```
//没有会安装，有就更新
bash <(curl -L -s https://install.direct/go.sh)

```

脚本运行完成后，你需要：

1. 编辑 /etc/v2ray/config.json 文件来配置你需要的代理方式(具体看配置config.json)-->`vi /etc/v2ray/config.json `;
2. 运行 service v2ray start 来启动 V2Ray 进程；
3. 之后可以使用 service v2ray start|stop|status|reload|restart|force-reload 控制 V2Ray 的运行。  
4. 其他使用到的命令

    ```
    sudo systemctl restart v2ray  
    service v2ray status  
    ```

# 时间校准

参考[Centos7修改系统时区timezone](https://blog.csdn.net/zlt995768025/article/details/79765738)  

```
timedatectl

//获取所有时区
timedatectl list-timezones
//设置时区
timedatectl set-timezone Asia/Shanghai
//重启
reboot
```

# [第三方客户端](https://v2ray.com/ui_client/)  

我下载的是[V2RayN](https://github.com/2dust/v2rayN)

# 服务端-->配置config.json

**简单生成方法：通过该[网站生成：v2ray配置文件生成器
](https://intmainreturn0.com/v2ray-config-gen/#])**

## 获得uuid

参考[官网VMess 协议](https://www.v2ray.com/developer/protocols/vmess.html)使用说明，到[该生成用户ID生成网站生成](https://www.uuidgenerator.net/)，通过上面v2ray配置生成器也有uuid生成

## 我自己根据官方改的

原版

```
{
  "inbounds": [{
    "port": 41180,//服务器监听端口
    "protocol": "vmess",//主传入协议
    "settings": {
      "clients": [
        {
          "id": "e0ee582a-7194-430e-8ca3-1cb44d8f96e9",// 用户ID，客户端与服务器必须相同
          "level": 1,
          "alterId": 64
        }
      ]
    }
  }],
  "outbounds": [{
    "protocol": "freedom",//主传出协议
    "settings": {}
  },{
    "protocol": "blackhole",//出站协议
    "settings": {},
    "tag": "blocked"
  }],
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": ["geoip:private"],
        "outboundTag": "blocked"
      }
    ]
  }
}

```

我改后

```
{
  "inbounds": [
    {
      "port": 设置端口号(1 ~ 65535),
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "通过网站生成uuid",
            "level": 1,
            "alterId": 你上面设置alterId
          }
        ]
      },
      "streamSettings": {
        "network": "mkcp",
        "kcpSettings": {
          "uplinkCapacity": 5,
          "downlinkCapacity": 100,
          "congestion": false,
          "header": {
            "type": "none"
          }
        }
      },
      "detour": {
        "to": "dynamicPort"
      }
    },
    {
      "protocol": "vmess",
      "port": "10000-20000",
      "tag": "dynamicPort",
      "settings": {
        "default": {
          "level": 1,
          "alterId": 你上面设置alterId的一半
        }
      },
      "allocate": {
        "strategy": "random",
        "concurrency": 2,
        "refresh": 3
      },
      "streamSettings": {
        "network": "mkcp",
        "kcpSettings": {
          "uplinkCapacity": 5,
          "downlinkCapacity": 100,
          "congestion": false,
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
      "settings": {
        
      }
    },
    {
      "protocol": "blackhole",
      "settings": {
        
      },
      "tag": "blocked"
    }
  ],
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "blocked"
      }
    ]
  }
}
```

# 测速命令

```
wget -qO- --no-check-certificate https://raw.githubusercontent.com/oooldking/script/master/superbench.sh | bash
```
