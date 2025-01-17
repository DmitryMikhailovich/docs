# Устранение ошибок при добавлении нового пользователя в облачную организацию

## Описание проблемы {#issue-description}
При попытке добавить нового пользователя в организацию отображается сообщение об ошибке:
```
text message: Permission Denied; status: 403; description: Permission Denied;
``` 
Добавление пользователя производится от имени владельца текущего облака или платежного аккаунта.

## Решение {#issue-resolution}

Добавить пользователя в организацию может администратор (роль `organization-manager.admin`) или владелец (роль `organization-manager.organizations.owner`) организации. Проверьте, имеется ли одна из этих ролей у аккаунта, от имени которого вы пытаетесь добавить в организацию нового пользователя.
Список пользователей организации вы можете найти на странице сервиса [Yandex Cloud Organization](https://org.cloud.yandex.ru/users)

{% note info %}

Процесс добавления ролей для аккаунтов внутри организации описан на странице [«Назначение прав доступа»](../../../organization/roles.md#admin).

{% endnote %}