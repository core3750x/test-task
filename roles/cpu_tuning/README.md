# cpu_tuning

Роль настраивает параметры CPU через TuneD.

Роль:

- устанавливает пакет `tuned`;
- создаёт отдельный TuneD-профиль;
- задаёт governor через переменную `cpu_tuned_governor`;
- ограничивает переходы CPU в C-state через `cpu_tuned_force_latency`;
- включает и запускает сервис TuneD;
- активирует профиль;
- запускает `tuned-adm verify` и выводит результат проверки.

Настройки по умолчанию находятся в `defaults/main.yml`. Для разных групп серверов governor можно переопределить в inventory или `group_vars`, например:

```yaml
cpu_tuned_governor: performance
cpu_tuned_force_latency: 0
```

Если виртуальная машина или конкретный CPU не предоставляет governor, TuneD может пропустить этот параметр. Роль не останавливается из-за отсутствия аппаратной поддержки governor: остальные доступные настройки профиля продолжают применяться, а результат `tuned-adm verify` выводится в конце выполнения.

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
