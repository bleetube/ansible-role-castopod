# Ansible Role: castopod

This Ansible Role installs a rootless [castopod](https://code.castopod.org/adaures/castopod) container using Podman. It is intended to be composed with separate roles for Podman, database, and web proxy.

## Requirements

* [podman](docs/PODMAN.md)
* [containers.podman](https://github.com/containers/ansible-podman-collections)

## Dependencies

* [mariadb](docs/DATABASE.md)
* redis
* [nginx_conf](docs/examples/nginx_conf.yml) (optional)

## Role Variables

See the role [defaults](defaults/main.yml) and the castopod [environment variable](https://docs.castopod.org/getting-started/docker.html#environment-variables) documentation. For a working example, see this [homelab stack](https://github.com/bleetube/satstack).

## Example Playbook

```yaml
- hosts: castopod
  roles:
    - role: nginxinc.nginx_core.nginx
      become: true
    - role: fauust.mariadb
      become: true
    - role: alvistack.podman
      become: true
    - role: bleetube.redis
      become: true
    - role: bleetube.castopod
      tags: castopod
  tasks:
    - import_tasks: nginx_conf.yml
```

## Systemd

```
systemctl --user status container-castopod.service
```

## Upgrades

Configure `castopod_version`.

```bash
ansible-playbook playbooks/castopod.yml --tags castopod
```

## Troubleshooting

```bash
podman logs --follow castopod
podman inspect castopod | jq .[].Config.Env
```
