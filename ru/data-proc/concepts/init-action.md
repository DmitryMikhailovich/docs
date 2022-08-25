# Скрипты инициализации

При [создании кластера](../operations/cluster-create.md) вы можете указать скрипты инициализации хостов. Это может быть полезно для автоматической установки или обновления ПО, необходимого для запуска [заданий](./jobs.md). Каждый скрипт будет выполнен от имени суперпользователя `root` только один раз при первом запуске хоста.

В первой строке файла скрипта укажите полный путь к интерпретатору, например, `#!/bin/sh` или `#!/usr/bin/python`.

URI скрипта может быть задан в схемах `https://`, `http://`, `hdfs://` и `s3a://`. Для `s3a://` требуется выполнение хотя бы одного из условий:

* [ACL бакета](../../storage/operations/buckets/edit-acl.md) должен разрешать сервисному аккаунту кластера операции чтения.
* Сервисному аккаунту кластера должна быть [присвоена роль](../../iam/operations/sa/assign-role-for-sa.md) `storage.viewer`.
* Доступ к бакету должен быть [публичным](../../storage/operations/buckets/bucket-availability.md).

## Переменные окружения {#env-variables}

Для использования в скриптах инициализации доступны следующие переменные окружения:

* `CLUSTER_ID` — идентификатор кластера.
* `S3_BUCKET` — имя привязанного бакета {{ objstorage-full-name }}.
* `ROLE` — роль хоста (`masternode`, `computenode` или `datanode`).
* `CLUSTER_SERVICES` — [список компонентов](../concepts/environment).
* `MAX_WORKER_COUNT` — максимальное количество хостов подкластеров для хранения и обработки данных.
* `MIN_WORKER_COUNT` — минимальное количество хостов подкластеров для хранения и обработки данных.

Например, чтобы выполнить часть скрипта только на управляющем хосте (`MASTERNODE`), используйте проверку значения переменной окружения `ROLE`:

```bash
if [[ "${ROLE}" == "masternode" ]]; then
   ...
fi
```

## Ошибки скриптов инициализации {#errors}

Если выполнение скрипта завершилось ошибкой:


1. Посмотрите логи в [{{ cloud-logging-full-name }}](../../logging/operations/read-logs.md) или на хостах кластера в файле `/var/log/yandex/dataproc-init-actions.log`.



1. Исправьте ошибку.
1. [Удалите](../operations/cluster-delete.md) этот кластер и [создайте](../operations/cluster-create.md) новый.