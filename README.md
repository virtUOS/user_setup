# Ansible Role to Add Unix User Accounts

This role creates user accounts and adds SSH keys.

## Role Variables

Have a look at the [defaults](defaults/main.yml) to see what variables you can set.

You will need to specify the variable `admins` as a list of usernames and SSH keys.
Keys can be URLs or local files:

```yaml
admins:
  - name: foo
    key: http://example.com/foo.pub
  - name: bar
    key: ssh-keys/bar.pub
```

## Example Playbook

Just add the role to your playbook and specify your template:

In your `requirements.yml`:
```yaml
- src: https://github.com/virtUOS/user_setup.git
  scm: git
  version: main
```

An example playbook to create two admin unsers and detele all other users:
```yaml
- hosts: all
  become: true
  roles:
    - role: user_setup
      delete_users: true
      admins:
        - name: foo
          key: http://example.com/foo.pub
        - name: bar
          key: ssh-keys/bar.pub
```


## Deleting Users

If `delete_users` is det to `true` (default), the role will try to delete all users not in `admins`.
Users are identified by the directories present in `/home`.
Users with no home directory or a home directory somewhere else are not touched by this role.


## License

[BSD-3-Clause](LICENSE)
