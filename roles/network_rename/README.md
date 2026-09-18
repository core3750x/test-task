# network_rename

Роль переименовывает активный сетевой интерфейс в `net0` через выбранный механизм: netplan или systemd `.link`-файл.

Роль:

- определяет активный интерфейс по facts маршрута по умолчанию;
- получает его MAC-адрес;
- при `network_rename_use_netplan: true` обновляет имя интерфейса в netplan-файлах;
- при `network_rename_use_netplan: false` создаёт правило `/etc/systemd/network/10-net0.link`;
- применяет netplan сразу в netplan-режиме;
- перезагружает сервер после изменения `.link` в link-режиме, если `network_rename_reboot: true`;
- после выполнения выводит имя интерфейса, MAC-адрес, IPv4-адрес и gateway.

Перед изменением роль проверяет наличие IPv4-маршрута по умолчанию, активного интерфейса и его MAC-адреса. Если хотя бы один из этих facts отсутствует, роль останавливается с ошибкой и не создаёт правило переименования.

Запуск:

```bash
ansible-playbook \
  -i inventory/hosts.yml \
  playbooks/prepare-network.yml
```

Проверка без изменений:

```bash
ansible-playbook \
  -i inventory/hosts.yml \
  playbooks/prepare-network.yml \
  --check --diff
```

В netplan-режиме конфигурация применяется сразу через `netplan apply`, перезагрузка не выполняется. В link-режиме перезагрузка выполняется только если `.link`-файл изменился. Её можно отключить переменной `network_rename_reboot: false`, но тогда новое имя применится после ручной перезагрузки или повторного создания интерфейса.

Для систем с netplan оставьте `network_rename_use_netplan: true`. Для систем без netplan задайте `network_rename_use_netplan: false`.

В текущем inventory для серверов с netplan значение задано в `inventory/group_vars/network_netplan_servers.yml`.

Для будущего сервера без netplan добавьте его в отдельную inventory-группу и создайте для неё `group_vars` со значением:

```yaml
network_rename_use_netplan: false
```

В этом режиме роль создаёт только systemd `.link`-файл.
