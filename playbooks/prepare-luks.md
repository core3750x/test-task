# prepare-luks.yml

Playbook использует роль `luks` для шифрования указанного в inventory block device через LUKS2, создания файловой системы, записи в `/etc/crypttab` и монтирования в `/data`.

Секрет загружается из зашифрованного Ansible Vault-файла `inventory/secrets/luks.yml`.

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
  --check --diff
```
