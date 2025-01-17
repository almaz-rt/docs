---
title: Управление резервными копиями (снапшотами) {{ OS }}
description: '{{ mos-name }} позволяет использовать механизм снапшотов {{ OS }} для управления резервными копиями данных. Для работы со снапшотами используется публичный API {{ OS }}, а для их хранения — бакет в {{ objstorage-name }}.'
keywords:
  - резервные копии OpenSearch
  - снапшоты OpenSearch
  - OpenSearch
---

# Управление резервными копиями в {{ mos-name }}

{{ mos-short-name }} позволяет создавать резервные копии [индексов](../concepts/indexing.md) как средствами {{ yandex-cloud }}, так и с помощью механизма [снапшотов](../../glossary/snapshot.md) {{ OS }}. Подробнее о механизме снапшотов см. в [документации {{ OS }}]({{ os.docs }}/opensearch/snapshots/snapshot-restore/).


## Резервное копирование средствами {{ yandex-cloud }} {#cloud-backups}

Вы можете создавать [резервные копии](../concepts/backup.md) и восстанавливать кластеры из имеющихся резервных копий.

Также {{ mos-name }} каждый час создает автоматическую резервную копию.

### Получить список резервных копий {#list-backups}

Вы можете получить список резервных копий, которые были созданы за последние 14 дней.

{% list tabs group=instructions %}

- Консоль управления {#console}

    Чтобы получить список резервных копий кластера:

    1. Перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-opensearch }}**.
    1. Нажмите на имя нужного кластера и выберите вкладку ![backups](../../_assets/console-icons/archive.svg) **{{ ui-key.yacloud.mdb.cluster.backups.label_title }}**.

    Чтобы получить список всех резервных копий в каталоге:

    1. Перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-opensearch }}**.
    1. Выберите вкладку ![backups](../../_assets/console-icons/archive.svg) **{{ ui-key.yacloud.mdb.cluster.backups.label_title }}**.

- CLI {#cli}

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы получить список всех резервных копий в каталоге, выполните команду:

    ```bash
    {{ yc-mdb-os }} backup list
    ```

    Результат:

    ```text
    +----------------------+---------------------+-------------------+---------------------+
    |          ID          |      CREATED AT     | SOURCE CLUSTER ID |      STARTED AT     |
    +----------------------+---------------------+-------------------+---------------------+
    | c9qlk4v13uq7******** | 2024-01-09 14:38:34 | c9qpm4i********   | 2024-01-09 14:38:28 |
    | c9qpm90p3pcg******** | 2024-01-09 13:38:31 | c9qpm4i********   | 2024-01-09 13:38:28 |
    +----------------------+---------------------+-------------------+---------------------+
    ```

    Если вы хотите ограничить список резервных копий, который выводится после запуска команды, передайте в команде флаг `--limit <количество_записей>`. Например, если вывод команды `{{ yc-mdb-os }} backup list` занимает несколько экранов, выполните команду `{{ yc-mdb-os }} backup list --limit 5`. Тогда вывод будет содержать список из последних пяти резервных копий.

- API {#api}

    Чтобы получить список резервных копий кластера, воспользуйтесь методом REST API [listBackups](../api-ref/Cluster/listBackups.md) для ресурса [Cluster](../api-ref/Cluster/index.md) или вызовом gRPC API [ClusterService/ListBackups](../api-ref/grpc/Cluster/listBackups.md) и передайте в запросе идентификатор кластера в параметре `clusterId`.

    {% include [get-cluster-id](../../_includes/managed-opensearch/get-cluster-id.md) %}

    Чтобы получить список резервных копий всех кластеров {{ mos-name }} в каталоге, воспользуйтесь методом REST API [list](../api-ref/Backup/list.md) для ресурса [Backup](../api-ref/Backup/index.md) или вызовом gRPC API [BackupService/List](../api-ref/grpc/Backup/list.md) и передайте в запросе идентификатор каталога в параметре `folderId`.

{% endlist %}

### Получить информацию о резервной копии {#get-backup}

{% list tabs group=instructions %}

- Консоль управления {#console}

    Чтобы получить информацию о резервной копии существующего кластера:

    1. Перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-opensearch }}**.
    1. Нажмите на имя нужного кластера и выберите вкладку ![backups](../../_assets/console-icons/archive.svg) **{{ ui-key.yacloud.mdb.cluster.backups.label_title }}**.

    Чтобы получить информацию о резервной копии удаленного ранее кластера:

    1. Перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-opensearch }}**.
    1. Выберите вкладку ![backups](../../_assets/console-icons/archive.svg) **{{ ui-key.yacloud.mdb.cluster.backups.label_title }}**.

- CLI {#cli}

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы получить информацию о резервной копии кластера:

    1. Получите идентификатор резервной копии вместе со списком всех резервных копий в каталоге:

        ```bash
        {{ yc-mdb-os }} backup list
        ```

        Идентификатор указан в столбце `ID` в выводе команды.

    1. Получите информацию о нужной резервной копии:

        ```bash
        {{ yc-mdb-os }} backup get <идентификатор_резервной_копии>
        ```

        Пример результата команды:

        ```text
        id: c9qlk4v13uq7********
        folder_id: {{ folder-id-example }}
        source_cluster_id: c9qpm4i********
        started_at: "2024-01-09T10:38:28.683Z"
        created_at: "2024-01-09T10:38:31.685Z"
        indices:
          - .mdb-sli
          - .opendistro_security
          - .kibana_1
          - .opendistro-job-scheduler-lock
          - .opensearch-observability
          - .opendistro-ism-config
        opensearch_version: 2.8.0
        indices_total: "6"
        ```

- API {#api}

    Чтобы получить информацию о резервной копии, воспользуйтесь методом REST API [get](../api-ref/Backup/get.md) для ресурса [Backup](../api-ref/Backup/index.md) или вызовом gRPC API [BackupService/Get](../api-ref/grpc/Backup/get.md) и передайте в запросе идентификатор резервной копии в параметре `backupId`.

    Чтобы узнать идентификатор резервной копии, [получите список резервных копий](#list-backups).

{% endlist %}

### Создать резервную копию {#create-backup}

{% list tabs group=instructions %}

- Консоль управления {#console}
  
    1. Перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-opensearch }}**.
    1. Нажмите на имя нужного кластера и выберите вкладку ![backups](../../_assets/console-icons/archive.svg) **{{ ui-key.yacloud.mdb.cluster.backups.label_title }}**.
    1. Нажмите кнопку ![image](../../_assets/console-icons/plus.svg) **{{ ui-key.yacloud.mdb.cluster.backups.button_create }}**.

    {% include [no-prompt](../../_includes/mdb/backups/no-prompt.md) %}

- CLI {#cli}

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы создать резервную копию данных кластера, выполните команду:

    ```bash
    {{ yc-mdb-os }} cluster backup <имя_или_идентификатор_кластера>
    ```

    Имя и идентификатор кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters).

- API {#api}

    Чтобы создать резервную копию, воспользуйтесь методом REST API [backup](../api-ref/Cluster/backup.md) для ресурса [Cluster](../api-ref/Cluster/index.md) или вызовом gRPC API [ClusterService/Backup](../api-ref/grpc/Cluster/backup.md) и передайте в запросе идентификатор кластера в параметре `clusterId`.

    {% include [get-cluster-id](../../_includes/managed-opensearch/get-cluster-id.md) %}

{% endlist %}

{% include [backup-warning](../../_includes/mdb/backups/backup-create-warning.md) %}

### Восстановить кластер из резервной копии {#restore}

Восстанавливая кластер из резервной копии, вы создаете новый кластер с данными из резервной копии. Если в каталоге не хватает [ресурсов](../concepts/limits.md) для создания такого кластера, восстановиться из резервной копии не получится.

Для нового кластера необходимо задать все параметры, обязательные при его создании.

{% list tabs group=instructions %}

- Консоль управления {#console}

    Чтобы восстановить из резервной копии существующий кластер:

    1. Перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-opensearch }}**.
    1. Нажмите на имя нужного кластера и выберите вкладку ![backups](../../_assets/console-icons/archive.svg) **{{ ui-key.yacloud.mdb.cluster.backups.label_title }}**.
    1. Нажмите на значок ![image](../../_assets/console-icons/ellipsis.svg) для нужной резервной копии, затем нажмите **{{ ui-key.yacloud.mdb.cluster.backups.button_restore }}**.
    1. Задайте настройки нового кластера.
    1. Нажмите кнопку **{{ ui-key.yacloud.mdb.forms.button_restore }}**.

    Чтобы восстановить из резервной копии удаленный ранее кластер:

    1. Перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-opensearch }}**.
    1. Выберите вкладку ![backups](../../_assets/console-icons/archive.svg) **{{ ui-key.yacloud.mdb.cluster.backups.label_title }}**.
    1. Найдите нужную резервную копию по времени создания и идентификатору кластера. В колонке **{{ ui-key.yacloud.common.id }}** содержатся идентификаторы в формате `<идентификатор_кластера>:<идентификатор_резервной_копии>`.
    1. Нажмите на значок ![image](../../_assets/console-icons/ellipsis.svg) для нужной резервной копии, затем нажмите **{{ ui-key.yacloud.mdb.cluster.backups.button_restore }}**.
    1. Задайте настройки нового кластера.
    1. Нажмите кнопку **{{ ui-key.yacloud.mdb.forms.button_restore }}**.

    {{ mos-name }} запустит операцию создания кластера из резервной копии.

- CLI {#cli}

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы восстановить кластер из резервной копии:

    1. Получите идентификатор резервной копии вместе со списком всех резервных копий в каталоге:

        ```bash
        {{ yc-mdb-os }} backup list
        ```

        Результат:

        ```text
        +----------------------+---------------------+-------------------+---------------------+
        |          ID          |      CREATED AT     | SOURCE CLUSTER ID |      STARTED AT     |
        +----------------------+---------------------+-------------------+---------------------+
        | c9qlk4v13uq7******** | 2024-01-09 14:38:34 | c9qpm4i********   | 2024-01-09 14:38:28 |
        | ...                                                                                  |
        +----------------------+---------------------+-------------------+---------------------+
        ```

        Идентификатор резервной копии указан в столбце `ID`. Время завершения создания резервной копии указано в столбце `CREATED AT` в формате `yyyy-mm-dd hh:mm:ss`.

    1. Запросите создание кластера из резервной копии:

        ```bash
        {{ yc-mdb-os }} cluster restore --backup-id <идентификатор_резервной_копии>
        ```

        В команде также можно передать параметры, которые задаются при создании кластера. Описание таких параметров читайте в разделе [Создание кластера](cluster-create.md).

- API {#api}

  Чтобы восстановить из резервной копии существующий кластер, воспользуйтесь методом REST API [restore](../api-ref/Cluster/restore.md) для ресурса [Cluster](../api-ref/Cluster/index.md) или вызовом gRPC API [ClusterService/Restore](../api-ref/grpc/Cluster/restore.md) и передайте в запросе:

    * Идентификатор нужной резервной копии в параметре `backupId`. Чтобы узнать идентификатор, [получите список резервных копий в кластере](#list-backups).
    * Имя нового кластера, который будет содержать восстановленные из резервной копии данные, в параметре `name`. Имя кластера должно быть уникальным в рамках каталога.
    * Конфигурацию кластера в параметре `configSpec`.
    * Идентификатор сети в параметре `networkId`.
    * Идентификатор каталога, в котором должен быть размещен кластер, в параметре `folderId`.

{% endlist %}

## Резервное копирование с помощью снапшотов {#snapshot-backups}

Для работы со снапшотами используется [публичный API {{ OS }}]({{ os.docs }}/api-reference/index/), а для их хранения — бакет в {{ objstorage-name }}.

### Получить список снапшотов {#list-snapshots}

1. Найдите в списке репозиториев {{ OS }} тот, который содержит в себе резервные копии в виде снапшотов:

    ```http
    GET https://admin:<пароль>@<идентификатор_хоста_{{ OS }}_с_ролью_DATA>.{{ dns-zone }}:{{ port-mos }}/_snapshot/_all
    ```

   Если нужного репозитория нет в списке, [подключите его](s3-access.md).

1. Получите список снапшотов в репозитории:

    ```http
    GET https://admin:<пароль>@<идентификатор_хоста_{{ OS }}_с_ролью_DATA>.{{ dns-zone }}:{{ port-mos }}/_snapshot/<имя_репозитория>/_all
    ```

   Каждой резервной копии соответствует один снапшот.

### Создать снапшот {#create-snapshot}

1. Найдите в списке репозиториев {{ OS }} тот, в котором нужно создать резервную копию в виде снапшота:

    ```http
    GET https://admin:<пароль>@<идентификатор_хоста_{{ OS }}_с_ролью_DATA>.{{ dns-zone }}:{{ port-mos }}/_snapshot/_all
    ```

    Если нужного репозитория нет в списке, [подключите его](s3-access.md).

1. [Создайте снапшот]({{ os.docs }}/opensearch/snapshots/snapshot-restore/#take-snapshots) нужных данных или целого кластера в выбранном репозитории:

    ```http
    PUT https://admin:<пароль>@<идентификатор_хоста_{{ OS }}_с_ролью_DATA>.{{ dns-zone }}:{{ port-mos }}/_snapshot/<имя_репозитория>/<имя_снапшота>
    ```

### Восстановить кластер из снапшота {#restore-from-snapshot}

{% note warning %}

При восстановлении из снапшота версия {{ OS }} в кластере должна быть не ниже версии {{ OS }}, в которой был сделан снапшот.

{% endnote %}

1. [Создайте новый кластер {{ OS }}](cluster-create.md) в нужной конфигурации, но не наполняйте его данными.

    При создании кластера выберите:

    * Количество и класс хостов, размер и тип хранилища исходя из размера снапшота и требований к быстродействию.

    * Версию {{ OS }}, в которой был создан снапшот, или более новую.

1. Закройте открытые индексы с помощью [{{ OS }} API]({{ os.docs }}/api-reference/index-apis/close-index/):

    ```http
    POST: https://admin:<пароль>@<идентификатор_хоста_{{ OS }}_с_ролью_DATA>.{{ dns-zone }}:{{ port-mos }}/<имя_индекса>/_close
    ```

    Для восстановления всего кластера закройте все открытые индексы. Для восстановления отдельных индексов закройте только их.

1. [Получите список резервных копий](#list-snapshots) и найдите нужный снапшот.
1. [Запустите операцию восстановления]({{ os.docs }}/opensearch/snapshots/snapshot-restore/#restore-snapshots) из нужного снапшота всего кластера или отдельных индексов и потоков данных.
