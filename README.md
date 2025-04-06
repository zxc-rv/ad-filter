# Ad-Filter 🚫✨

Списки доменов с рекламой на базе Hagezi Pro для использования с клиентами на базе Xray и Sing-Box. Ежедневное обновление.

![image](https://github.com/user-attachments/assets/626c5ead-f456-4817-b0ae-21e8a8abef81)


## Последние версии
Скачивайте самые свежие списки в любое время:

- 📦 https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.dat [Xray]
- 📦 https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.srs [Sing-Box]

## Как это работает
1. **Источник данных**: Загружаются домены из wildcard-списка Hagezi Pro.
2. **Обработка**: Удаляются комментарии, форматируются данные и компилируются в нужные форматы.
3. **Автоматизация**: GitHub Actions запускается ежедневно для создания и публикации файлов.
4. **Доставка**: Файлы публикуются как активы релиза GitHub для удобного доступа.

## Примеры конфигураций:

### Для Xray (c версии 1.8.7)
Загрузите `adlist.dat` в нужную директорию и добавьте правило для него:

```json
{
  "routing": {
    "rules": [
      {
        "domain": "ext:adlist.dat:hagezi-pro",
        "outboundTag": "block"
      }
    ]
  }
}
```
> [!TIP]
> При использовании XKeen на роутерах Keenetic можно скачать командой в entware:
> ```
> curl -L -o /opt/etc/xray/dat/adlist.dat https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.dat
> ```
> Также можно добавить автоматизацию Cron:
> ```
> echo "3 5 * * * /opt/bin/curl -L -o /opt/etc/xray/dat/adlist.dat https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.dat && xkeen -restart" >> /opt/var/spool/cron/crontabs/root
> ```

### Для Sing-Box (с версии 1.11.0)
Добавьте rule_set в конфигурацию Sing-Box и правило для него:

```json
{
  "route": {
    "rules": [
      {
        "rule_set": "ads",
        "action": "reject"
      }
    ],
    "rule_set": [
      {
        "tag": "ads",
        "type": "remote",
        "format": "binary",
        "url": "https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.srs",
        "download_detour": "direct"
      }
    ]
  }
}
```


