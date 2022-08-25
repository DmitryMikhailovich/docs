# Сеть в {{ data-transfer-full-name }}

При создании эндпоинта вы можете выбрать [облачную сеть](../../vpc/concepts/network.md) и [группы безопасности](../../vpc/concepts/security-groups.md) для сетевого трафика. Если используемые в эндпоинтах виртуальные машины или кластеры БД находятся в выбранной облачной сети, то во время трансфера к ним будут применены правила выбранных групп безопасности. Это позволяет выполнять трансферы, не изменяя правила существующих групп безопасности виртуальных машин или кластеров БД.

Если вам необходимо мигрировать данные между {{ yandex-cloud }} и сторонним облаком, добавьте в группу безопасности кластера-эндпоинта правила, разрешающие входящие подключения к нему из интернета с [IP-адресов, используемых сервисом {{ data-transfer-name }}](https://stat.ripe.net/widget/announced-prefixes#w.resource%3DAS200350%26w.min_peers_seeing%3D0).

## Диапазоны IP-адресов подсетей {#subnet-address-ranges}

При трансфере между хостами источника и приемника, находящимися внутри {{ yandex-cloud }} в разных подсетях, диапазоны IP-адресов этих подсетей не должны пересекаться. Например, к ошибке приведет использование хостами подсетей с такими диапазонами:

* `network-1/subnet-a` с IPv4 CIDR `10.130.0.0/24`.
* `network-2/subnet-b` с IPv4 CIDR `10.130.0.0/24`.

## Трансфер между источником из внешней сети и приемником в {{ yandex-cloud }} {#source-external}

Обеспечить доступ к источнику, который находится во внешней сети, можно одним из следующих способов:

* [настроив источник таким образом, чтобы к нему можно было подключиться из интернета](../operations/prepare.md#source-mg).
* с помощью [{{ interconnect-full-name }}](../../interconnect/index.yaml);
* с помощью промежуточной ВМ, на которой настроена [маршрутизация трафика в {{ vpc-name }}](../../vpc/concepts/static-routes.md).


## Скорость копирования {#copy-speed}

При копировании скорость может достигать 15 МБ/с. База размером 100 ГБ обычно копируется за 2–3 часа. Точное время зависит от настроек приемника и источника, а также от стабильности соединения.

При репликации {{ PG }} и {{ MY }} пропускная способность может достигать 20–30 тысяч пишущих транзакций в секунду.