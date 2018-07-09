# MTProto

MTProto proxy - специальный прокол для Telegram. Он состоит из пары входящих и исходящих прокси в V2Ray. Они обычно используются вместе для создания прокси для Telegram.

**На данный момент V2Ray поддерживает только IPv4 адрес сервера Telegram.**

Описание протокола:

* Название: mtproto
* Тип: входящий / исходящий

## Конфигурация входящего соединения {#inbound}

```javascript
{
  "users": [{
    "email": "love@v2ray.com",
    "level": 0,
    "secret": "b0cbcef5a486d9636472ac27f8e11a9d"
  }]
}
```

Где:

* `users`: Массив пользователей. **На данный момент поддерживается только первый пользователь**. Каждый пользователь имеет следующую конфигурацию: 
  * `email`: Электронная почта пользователя. Используется для сбора статистики. См. [ Статистика ](../stats.md).
  * ` userLevel `: Пользовательский уровень.
  * `secret`: Секрет пользователя. В Telegram секрет пользователя должен быть длиной 32 символа и содержать только символы ` 0 ` — ` 9 `, и ` a ` — ` f `.

## Конфигурация исходящего соединения {#outbound}

```javascript
{
}
```

## Пример {#sample}

MTProto может использоваться только для трафика Telegram. Для объединения соответствующего входящего и исходящего может потребоваться правило маршрутизации. Here is an incomplete sample.

Inbound:

```javascript
{
  "tag": "tg-in",
  "port": 443,
  "protocol": "mtproto",
  "settings": {
    "users": [{"secret": "b0cbcef5a486d9636472ac27f8e11a9d"}]
  }
}
```

Outbound:

```javascript
{
  "tag": "tg-out",
  "protocol": "mtproto",
  "settings": {}
}
```

Routing:

```javascript
{
  "type": "field",
  "inboundTag": ["tg-in"],
  "outboundTag": "tg-out"
}
```

The configure your Telegram app to connect to 443 port on this machine.

## Tips {#tips}

* Use this command to generate MTProto secret: `openssl rand -hex 16`.