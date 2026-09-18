# test-task

## control node

- Ansible community package: 14.4.0
- ansible-core: 2.21.4
- Python: 3.14.7
- platform: macOS arm64

## Ansible collections

Ansible modules are versioned as part of their collections. The project uses the following collection versions:

- `ansible.posix`: 2.2.2
- `community.crypto`: 3.4.0
- `community.general`: 13.4.0

## Planned modules

- `community.crypto.luks_device` for LUKS operations
- `community.general.filesystem` for filesystem creation
- `ansible.posix.mount` for persistent mounts
- `ansible.builtin.apt` for package installation
- `ansible.builtin.file` for directories and files
- `ansible.builtin.assert` for validation
- `ansible.builtin.debug` for execution results and the final report
