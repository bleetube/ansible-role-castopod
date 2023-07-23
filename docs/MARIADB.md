# Mariadb

Here is an example using [fauust.mariadb](https://github.com/fauust/ansible-role-mariadb).

## Example Playbook

```yaml
  roles:
    - fauust.mariadb
```

## Example Variables

```yaml
mariadb_databases:
  - name: castopod
    collation: utf8_general_ci
    encoding: utf8
    replicate: false

mariadb_users:
  - name: castopod
    host: localhost
    password: "{{ lookup('ansible.builtin.env', 'CASTOPOD_MARIADB') }}"
    priv: "castopod.*:ALL"
    state: present
  - name: castopod
    host: '%'
    password: "{{ lookup('ansible.builtin.env', 'CASTOPOD_MARIADB') }}"
    priv: "castopod.*:ALL"
    state: present
```

In this example, there are two users because both `localhost` and `%` (all-hosts wildcard) are [mutually exclusive](https://stackoverflow.com/q/10823854/9290). I am also using environment variables to  separate secret stores from the repository.