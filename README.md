# ansible_cribl_edge

Ansible Role to deploy Cribl Edge

## Dependencies

### Air-Gapped Environments

- HTTP File Server Hosting the:
  - Installation package from [Cribl](https://cribl.io/download/)
  - Installation packge MD5 Checksum file

## Variables

| Variable                        | Default Value                       | Notes                                                                              |
| ------------------------------- | ----------------------------------- | ---------------------------------------------------------------------------------- |
| cribl_edge_os                   | linux-x64                           | Used to help set Download URL                                                      |
| cribl_edge_version              | 4.10.0-22f23292                     | Used to help set Download URL                                                      |
| cribl_edge_airgapped            | false                               | Determines whether the Cribl Install package be will downloaded a local fileserver |
| cribl_edge_airgapped_location   | `https://filesrv.local.domain`      | Used to help set Download URL                                                      |
| cribl_edge_leader               | `https://cribl-leader.local.domain` | Cribl Leader Web GUI                                                               |
| cribl_edge_leader_communication | cribl-leadercomm.local.domain       | Cribl Leader Communication URL                                                     |
| cribl_edge_leader_comm_port     | 4200                                | Cribl Leader Communication Port                                                    |
| cribl_edge_leader_tls_disabled  | true                                | Cribl Leader Communication TLS Enabled/Disabled                                    |
| cribl_edge_fleet                | default_fleet                       | Name of Fleet to put the Edge nodes into                                           |
| cribl_edge_token                | {{ vault_cribl_edge_token }}        | Token set during Cribl Leader Installation                                         |
| cribl_edge_user                 | cribl                               | Username to run Cribl Edge as                                                      |
| cribl_edge_group                | cribl                               | Group of Cribl user that Edge runs as                                              |
| cribl_edge_install_dir          | /opt/cribl                          | Location to install Cribl Edge                                                     |

### Vaulted Variables

- `vault_cribl_edge_token` can be found in vars/vault.yml and should be encrypted with ansible-vault before being used

## Inventory Example

```yaml
---
linux:
  vars:
    cribl_edge_os: linux-x64
    cribl_edge_version: 4.10.0-22f23292
    cribl_edge_airgapped_location: https://filesrv.local.domain
    cribl_edge_leader: https://cribl-leader.local.domain
    cribl_edge_leader_communication: cribl-leadercomm.local.domain
    cribl_edge_fleet: linux
  hosts:
    rhelvm01.local.domain:
      ansible_hosts: 10.10.1.20
      cribl_edge_airgapped: false
    rhelvm02.local.domain:
      ansible_hosts: 10.10.1.30
      cribl_edge_airgapped: true
```

## Playbook Example

```yaml
---
- name: Configure Cribl Edge
  hosts: all
  become: true
  roles:
    - { role: ansible_cribl_edge }
```

## Role Usage & Tags

- The `install` tag is used to install and configure Cribl Edge application

```bash
ansible-playbook -i inventory.yml configure-cribl-edge.yml --tag install -K
```

- The `uninstall` tag is used to uninstall Cribl Edge application including removing any created users

```bash
ansible-playbook -i inventory.yml configure-cribl-edge.yml --tag uninstall -K
```
