MBzeGuard — блокировка нежелательных ресурсов прямо на маршрутизаторе

Помогаем провайдерам в условиях дефицита сетевого оборудования: блокируем нежелательные ресурсы прямо на своих роутерах, тем самым снижаем нагрузку на сеть и улучшаем стабильность подключения.

Зарубежные сервисы должны знать: мы справимся и без них — сами решим, что нам нужно, а что нет.
📦 Поддерживаемые форматы списков

    Dnsmasq (nfset) — для OpenWrt ≥ 23.05
    nftset=/showip.net/4#inet#fw4#vpn_domains

    Dnsmasq (ipset) — для OpenWrt ≤ 21.02
    ipset=/showip.net/vpn_domains

    Sing-box Source — начиная с версии 1.11.0

    Xray Dat — общий geosite.dat с категориями

    ClashX
    DOMAIN-SUFFIX,showip.net

    Mikrotik FWD
    /ip dns static add name=fast.com type=FWD...

    Kvas — отсортированный список доменов для Kvas 1.1.8+

    RAW — простой список без формата

Пример конфигурации Dnsmasq

Dnsmasq автоматически помещает IP-адреса резолвленных доменов в ipset vpn_domains. Далее этот список можно использовать для фильтрации, например, полностью заблокировав доступ к этим IP.
📂 Списки по категориям, сервисам и странам
📁 Категории:

    Anime

    Block

    GeoBlock

    News

    Porn

🌐 Сервисы:

    Cloudflare

    Discord

    HDRezka

    Meta*

    Telegram

    TikTok

    Twitter

    YouTube

🌍 Страны:
🇷🇺 Россия

    Russia inside — включает зарубежные ресурсы, блокирующие российские IP, и нежелательные категории контента.

    Russia outside — российские ресурсы, доступные только из РФ. Полезно тем, кто за пределами страны и хочет получить доступ.

🇺🇦 Украина

Списки с заблокированными в стране ресурсами, основаны на данных:

    https://uablacklist.net/

    https://zaborona.help/

🔗 Прямые ссылки на списки

Все списки доступны по этой ссылке
Общий файл для Xray:
geosite.dat

Остальные списки доступны в формате RAW, dnsmasq, ClashX, Kvas, Mikrotik и др.
Развёрнутый список — под спойлерами (см. ниже в репозитории).
🛠️ Как применить на роутере (OpenWrt 23.05+)

cd /tmp/ && opkg download dnsmasq-full
opkg remove dnsmasq && opkg install dnsmasq-full --cache /tmp/
cp /etc/config/dhcp /etc/config/dhcp-old && mv /etc/config/dhcp-opkg /etc/config/dhcp

cd /tmp/dnsmasq.d && wget https://raw.githubusercontent.com/itdoginfo/allow-domains/main/Russia/inside-dnsmasq-nfset.lst -O domains.conf

uci add firewall ipset
uci set firewall.@ipset[-1].name='vpn_domains'
uci set firewall.@ipset[-1].match='dst_net'

uci add firewall rule
uci set firewall.@rule[-1].name='block_domains'
uci set firewall.@rule[-1].src='lan'
uci set firewall.@rule[-1].dest='*'
uci set firewall.@rule[-1].proto='all'
uci set firewall.@rule[-1].ipset='vpn_domains'
uci set firewall.@rule[-1].family='ipv4'
uci set firewall.@rule[-1].target='DROP'

uci commit
service firewall restart && service dnsmasq restart

📥 Как добавить домены в списки?

Принимаются Pull Requests и обсуждения в разделе Discussions.
Для каждого списка есть отдельная ветка обсуждений — оформляйте по правилам.
🌍 Добавить новую страну или формат?

Да, вы можете:
Страна:

    Укажите название страны

    Приложите список ресурсов

    При наличии — дайте ссылку на существующий источник

Формат:

    Пример формата

    Где и как он используется

    Способ тестирования списка (если есть)

📢 Канал с обновлениями

Подписывайтесь:
https://t.me/MBzeGuard