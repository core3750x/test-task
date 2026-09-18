# prepare-network.yml

Playbook использует роль `network_rename` для группы `network_netplan_servers`. Роль определяет активный интерфейс по IPv4-маршруту по умолчанию, применяет выбранный механизм переименования, перезагружает сервер после изменения конфигурации и выводит сетевые facts.

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
