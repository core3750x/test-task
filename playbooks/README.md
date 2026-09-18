# playbooks

В каталоге находятся независимые playbook-и:

- [prepare-luks.md](prepare-luks.md)
- [prepare-cpu-tuning.md](prepare-cpu-tuning.md)
- [prepare-network.md](prepare-network.md)

Каждый playbook требует `become: true` и собирает facts перед выполнением роли.
