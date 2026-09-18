# test-task

## Компоненты

- Ansible community package: 14.4.0
- ansible-core: 2.21.4
- Python: 3.14.7
- Платформа: macOS arm64

## Коллекции Ansible

Версии модулей Ansible определяются версиями коллекций. В проекте используются следующие версии:

- `ansible.posix`: 2.2.2
- `community.crypto`: 3.4.0
- `community.general`: 13.4.0

Версии зафиксированы в `collections/requirements.yml`. Установить коллекции можно командой:

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

## Используемые модули

- `community.crypto.luks_device` — операции с LUKS;
- `community.general.crypttab` — управление `/etc/crypttab`;
- `community.general.filesystem` — создание файловой системы;
- `ansible.posix.mount` — постоянное монтирование;
- `ansible.builtin.apt` — установка пакетов;
- `ansible.builtin.package` и `ansible.builtin.service` — установка и запуск TuneD;
- `ansible.builtin.file` — создание каталогов и файлов;
- `ansible.builtin.copy` и `ansible.builtin.replace` — управление конфигурационными файлами;
- `ansible.builtin.find` и `ansible.builtin.setup` — поиск конфигурации и сбор сетевых facts;
- `ansible.builtin.reboot` — перезагрузка для применения systemd `.link`;
- `ansible.builtin.assert` — проверки;
- `ansible.builtin.debug` — вывод результатов выполнения и итогового отчёта.

Роли используют `ansible.builtin.command` только для `tuned-adm` и `netplan apply`. Для этих утилит нет соответствующих модулей в зафиксированных коллекциях; вызовы выполняются с явными условиями идемпотентности.
