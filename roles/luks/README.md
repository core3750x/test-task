# luks

Роль создаёт LUKS2-контейнер на указанном диске или разделе, открывает его, создаёт файловую систему и монтирует её.

Устройство задаётся в inventory. Роль подходит для целого второго диска и для отдельного раздела, который не содержит root-файловую систему.

## Используемые модули

- `community.crypto.luks_device` — создание и открытие LUKS2;
- `community.general.crypttab` — запись устройства в `/etc/crypttab`;
- `community.general.filesystem` — создание файловой системы;
- `ansible.posix.mount` — постоянное монтирование через `/etc/fstab`;
- `ansible.builtin.apt` — установка `cryptsetup` и `e2fsprogs`;
- `ansible.builtin.stat` — проверка устройства;
- `ansible.builtin.assert` — проверки входных данных и безопасности операции;
- `ansible.builtin.file` — создание точки монтирования.

## Переменные

Обязательные переменные:

```yaml
luks_device: /dev/vdb
luks_passphrase: "пароль"
```

Значения по умолчанию:

```yaml
luks_mapper_name: data_crypt
luks_mount_point: /data
luks_filesystem: ext4
luks_confirm: false
luks_packages:
  - cryptsetup
  - e2fsprogs
```

`luks_confirm` нужно установить в `true`, чтобы разрешить изменение устройства.

Содержимое выбранного устройства будет уничтожено. Указывайте только пустой диск или раздел без важных данных.

Passphrase LUKS не хранится в открытом виде. Он находится в зашифрованном файле:

```text
inventory/secrets/luks.yml
```

Файл создаётся командой:

```bash
ansible-vault create inventory/secrets/luks.yml
```

В файл записывается:

```yaml
vault_luks_passphrase: "пароль-для-LUKS"
```

Связь с переменной роли задана в файле `inventory/luks_vars.yml`:

```yaml
luks_passphrase: "{{ vault_luks_passphrase }}"
luks_confirm: true
```

Посмотреть расшифрованное содержимое можно командой:

```bash
ansible-vault view inventory/secrets/luks.yml
```

## Пример inventory

```yaml
all:
  children:
    prepared_servers:
      hosts:
        test-vm01:
          luks_device: /dev/vdb
```

Переменные passphrase и подтверждения можно добавить в group vars:

```yaml
luks_passphrase: "{{ vault_luks_passphrase }}"
luks_confirm: true
```

## Запуск

Роль подключается в `playbooks/prepare-luks.yml`:

```yaml
- name: prepare encrypted storage
  hosts: prepared_servers
  become: true
  gather_facts: true

  vars_files:
    - ../inventory/secrets/luks.yml
    - ../inventory/luks_vars.yml

  roles:
    - role: luks
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

Реальный запуск:

```bash
ansible-playbook \
  -i inventory/hosts.yml \
  playbooks/prepare-luks.yml \
  --ask-vault-pass
```

## Проверки роли

Перед изменением устройства роль проверяет:

- наличие `luks_device` и passphrase;
- наличие устройства и его тип block device;
- наличие root mount среди собранных facts;
- отсутствие mount на выбранном устройстве;
- что выбранное устройство не является root-разделом или root-диском;
- что destructive-операция разрешена через `luks_confirm`.

Playbook должен запускаться с `gather_facts: true`.

После выполнения роль создаёт в `/etc/crypttab` запись с устройством, но без keyfile:

```text
data_crypt /dev/vdb
```

Поэтому после перезагрузки VM система может запросить passphrase для открытия контейнера. Файловая система монтируется в указанную точку, по умолчанию `/data`.
