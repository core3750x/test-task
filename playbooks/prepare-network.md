# prepare-network.yml

Playbook использует роль `network_rename`. Роль определяет активный интерфейс по IPv4-маршруту по умолчанию, создаёт systemd `.link`-правило для имени `net0`, перезагружает сервер после изменения правила и выводит сетевые facts.

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
