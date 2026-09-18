# prepare-cpu-tuning.yml

Playbook использует роль `cpu_tuning`. Роль устанавливает TuneD, создаёт CPU-профиль, задаёт governor и `force_latency`, запускает сервис и проверяет активный профиль.

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
  --check --diff
```
