# ChatGPT Telegram Bot

## Docker 本地部署

```bash
docker run -p 80:8080 -dit \
    -e BOT_TOKEN="telegram bot token" \
    -e WEB_HOOK="https://your_host.com/" \
    -e email="openai-chatgpt-email" \
    -e password="openai-chatgpt-password" \
#    -e session_token="123" \
    chatgpt:1.0
```

- MODE: 可选，设置生产环境/开发环境， 默认生产环境。可选值：'dev'|'prod'。
- BOT_TOKEN: 你需要在 [BotFather](https://t.me/BotFather) 创建一个 bot 以获取 BOT_TOKEN 。
- WEB_HOOK: 参考 [webhook](https://core.telegram.org/bots/api#setwebhook)，不包含 BOT_TOKEN 。（例如：``https://fly-app-name.fly.dev/``）
- NICK: 可选，NICK 是机器人的名字。当用户输入消息以 NICK 开头，机器人才会回答，否则机器人会回答任何消息。尤其在群聊里，没有 NICK，机器人会对所有消息进行回复。
- session_token: ChatGPT 的 cookie 中 `__Secure-next-auth.session-token` 的值
- email，password: openai 的账号密码，如果使用 session_token，就不需要传入邮箱密码。

或者你想使用 Docker Compose，下面是 docker-compose.yml 示例:

```yaml
version: "3.5"
services:
  chatgptbot:
    container_name: chatgptbot
    image: yym68686/chatgpt:1.0
    environment:
      - NICK=
      - BOT_TOKEN=
      - WEB_HOOK=
      - EMAIL=
      - PASSWORD=
      # - session_token=
    ports:
      - 80:8080
```

## fly.io 远程部署

官方文档：https://fly.io/docs/

使用 Docker 镜像部署 fly.io 应用

```bash
flyctl launch --image yym68686/chatgpt:1.0
```

输入应用的名字，若提示初始化 Postgresql 或 Redis，一律选择否。

按照提示部署。在官网控制面板会提供一个二级域名，可以使用这个二级域名访问到服务。

设置环境变量

```bash
flyctl secrets set WEB_HOOK=https://flyio-app-name.fly.dev/
flyctl secrets set BOT_TOKEN=bottoken
flyctl secrets set EMAIL=
flyctl secrets set PASSWORD=
# flyctl secrets set session_token=

# 可选
flyctl secrets set NICK=javis
```

查看所有环境变量

```bash
flyctl secrets list
```

移除环境变量

```bash
flyctl secrets unset MY_SECRET DATABASE_URL
```

ssh 连接 fly.io 容器

```bash
# 生成密钥
flyctl ssh issue --agent
# ssh 连接
flyctl ssh establish
```

查看 webhook url 是否正确

```
https://api.telegram.org/bot<token>/getWebhookInfo
```

## Reference

参考项目：

https://github.com/acheong08/ChatGPT

https://github.com/franalgaba/chatgpt-telegram-bot-serverless
