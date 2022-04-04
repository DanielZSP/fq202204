# fq202204



## 一键部署

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Github Actions 管理

请 Fork 本项目到自己的账户下。 Actions 需要以下 Secrets 才能正常工作，这些 Secrets 会被 workflow 中的 [akhileshns/heroku-deploy](https://github.com/AkhileshNS/heroku-deploy) 使用。

具体实现细节，请查看 [workflow 配置文件](./.github/workflows/main.yml).

| Name              | Description                                |
| ----------------- | ------------------------------------------ |
| APP_NAME          | 就是你 heroku 项目的名字                   |
| EMAIL             | heroku 账户的 email                        |
| HEROKU_API_KEY    | heroku API key， 在 account 设置下可以找到 |
| HEROKU_V2RAY_UUID | V2rayUUID                                  |

> 请务必生成新的 UUID。使用已有的 UUID 会使自己 V2ray 暴露在危险之下。

PowerShell:

```powershell
PS C:\Users\> New-Guid
```

Shell:

```bash
xxx@xxx:/mnt/c/Users/$ uuidgen
```

### Github Secrets

路径

```text
项目Setting-->Secrets
```

![Secrets](./readme-data/GithubSecrets.gif)

### Heroku API key

路径

```text
heroku Account settings-->API key
```

![Secrets](./readme-data/herokuapikey.gif)

### Github Actions 界面

```text
Actions
```

![Actions](./readme-data/githubactions.gif)

### 重新部署

点击 `Run workflow`, 输入 deploy。 然后就会重新 deploy。

![deploy](./readme-data/deploy.jpg)

### 停止

点击 `Run workflow`, 输入 stop。 然后就会 stop，不在计入小时数。
![stop](./readme-data/stop.jpg)

### 启动

点击 `Run workflow`, 输入 start。 然后就会启动。

![start](./readme-data/start.jpg)


## 建立 cloudflare worker （可选）

如果遇到创建的app在正常网络下不能访问，请尝试这个。

可以参考 开头的视频。代码如下。

```javascript
addEventListener("fetch", (event) => {
  let url = new URL(event.request.url);
  url.hostname = "你的heroku的hostname";
  let request = new Request(url, event.request);
  event.respondWith(fetch(request));
});
```

为 worker 选择速度更快的 IP。
https://github.com/badafans/better-cloudflare-ip

## 使用 Environments 实现 多账户/多app Secrets 管理

文档介绍： https://docs.github.com/en/actions/deployment/using-environments-for-deployment

### 建立 Environments, 并添加 Secrets

1. 创建 Environments
![Environments](./readme-data/Environments.png)
2. 添加 Secrets
![EnvironmentsSercet](./readme-data/EnvironmentsSercet.png)

### 输入环境名字
**一定要确保环境名字是对的，要不然就会用主的Secrets。**
![EnvironmentsDeploy](./readme-data/EnvironmentsDeploy.png)

## VLESS websocket 客户端配置

### JSON

```json
"outbounds": [
        {
            "protocol": "vless",
            "settings": {
                "vnext": [
                    {
                        "address": "***.herokuapp.com", // heroku app URL 或者 cloudflare worker url/ip
                        "port": 443,
                        "users": [
                            {
                                "id": "", // 填写你的 UUID
                                "encryption": "none"
                            }
                        ]
                    }
                ]
            },
            "streamSettings": {
                "network": "ws",
                "security": "tls",
                "tlsSettings": {
                    "serverName": "***.herokuapp.com" // heroku app host 或者 cloudflare worker host
                }
              }
          }
    ]
```

### v2rayN

换成 [V2rayN](https://github.com/2dust/v2rayN)

别人的配置教程参考，https://v2raytech.com/v2rayn-config-tutorial/.

![v2rayN](/readme-data/V2rayN.jpg)
