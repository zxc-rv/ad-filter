# Ad-Filter 🚫✨

Списки доменов с рекламой на базе Hagezi Pro для использования с клиентами на базе Xray, Sing-Box и Mihomo.

![image](https://github.com/user-attachments/assets/626c5ead-f456-4817-b0ae-21e8a8abef81)


## Статичные ссылки
Скачивайте самые свежие списки в любое время:

- 🩻 Xray https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.dat
- 📦 Sing-Box https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.srs
- 😼 Mihomo https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.mrs
## Как это работает
1. Загружаются домены из wildcard-списка [Hagezi Pro](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/pro-onlydomains.txt).
2. Удаляются комментарии, форматируются данные и компилируются в нужные форматы.
3. GitHub Action запускается ежедневно для генерации свежих файлов из источника.
4. Файлы публикуются как активы релиза GitHub для удобного доступа.

## Примеры конфигураций

### Xray (c версии 1.8.7)
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
> При использовании XKeen с Xray на роутерах Keenetic можно скачать командой в entware:
> ```
> curl -Lfo /opt/etc/xray/dat/adlist.dat https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.dat
> ```
> Автоматизация Cron (автообновление ежедневно в 5 утра + перезагрузка XKeen):
> ``` 
> echo "0 5 * * * /opt/bin/curl -Lfo /opt/etc/xray/dat/adlist.dat https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.dat && xkeen -restart" >> /opt/var/spool/cron/crontabs/root
> ```

### Sing-Box (с версии 1.11.0)
Добавьте rule_set в конфигурацию Sing-Box и правило для него:

```json
{
  "route": {
    "rule_set": [
      {
        "tag": "ads",
        "type": "remote",
        "format": "binary",
        "url": "https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.srs",
        "download_detour": "direct"
      }
    ],
    "rules": [
      {
        "rule_set": "ads",
        "action": "reject"
      }
    ]
  }
}
```

### Mihomo
Добавьте rule-set в конфигурацию Mihomo и правило для него:

```yaml
rule-providers:
  adfilter:
    type: http
    behavior: domain
    format: mrs
    url: https://github.com/zxc-rv/ad-filter/releases/latest/download/adlist.mrs
    path: ./rule-providers/adlist.mrs
    interval: 86400
rules:
  - RULE-SET,adfilter,REJECT
```


