# playbooks

В каталоге находятся два playbook-а. Они разделены, чтобы запускать LUKS и CPU-настройки независимо друг от друга.

## prepare-luks.yml

Использует роль `luks`.

Роль:

- проверяет выбранное block device;
- защищает root-раздел и root-диск от выбора;
- создаёт и открывает LUKS2;
- создаёт файловую систему;
- добавляет запись в `/etc/crypttab`;
- монтирует устройство через `/etc/fstab`.

Playbook загружает зашифрованные LUKS-переменные из:

```text
inventory/secrets/luks.yml
```

Запуск:

```bash
ansible-playbook \
  -i inventory/hosts.yml \
  playbooks/prepare-luks.yml \
  --ask-vault-pass
```

Проверка без изменений:

```bash
ansible-playbook \
  -i inventory/hosts.yml \
  playbooks/prepare-luks.yml \
  --ask-vault-pass \
  --check \
  --diff
```

## prepare-cpu-tuning.yml

Использует роль `cpu_tuning`.

Роль:

- устанавливает TuneD;
- создаёт отдельный профиль только для CPU;
- устанавливает governor `performance`;
- ограничивает переходы в C-state через `force_latency`;
- включает и запускает сервис TuneD;
- проверяет активный профиль.

Запуск:

```bash
ansible-playbook \
  -i inventory/hosts.yml \
  playbooks/prepare-cpu-tuning.yml
```

Проверка без изменений:

```bash
ansible-playbook \
  -i inventory/hosts.yml \
  playbooks/prepare-cpu-tuning.yml \
  --check \
  --diff
```

`prepare-luks.yml` использует группу `prepared_servers`, а `prepare-cpu-tuning.yml` — группу `cpu_performance_servers` из `inventory/hosts.yml`. Каждый playbook собирает facts один раз перед выполнением роли и требует `become: true`.
