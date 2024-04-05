# Ansible Role to Add Unix User Accounts

> ‼️  This role is made specifically for Osnabrück University.

This role creates user accounts and adds SSH keys.

## Dependencies

This role requires the general community collection:

```
ansible-galaxy collection install community.general
```

## Role Variables

Have a look at the [defaults](defaults/main.yml) to see what variables you can set.

You will need to specify the variable `admins` as a list of usernames.

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
      admins: [lolek, bolek]
      delete_users: true
```


## Deleting Users

If `delete_users` is det to `true` (default), the role will try to delete all users not in `admins`.
Users are identified by the directories present in `/home`.
Users with no home directory or a home directory somewhere else are not touched by this role.


## License

[BSD-3-Clause](LICENSE)
