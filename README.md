# Ansible Role to Add Unix User Accounts

This role creates user accounts and adds SSH keys.

## Requirements

This role requires the `ansible.posix` collection.
Install it via:

```
ansible-galaxy collection install ansible.posix
```

## Role Variables

Have a look at the [defaults](defaults/main.yml) to see what variables you can set.

You will need to specify the variable `admins` as a list of usernames and SSH keys.
Public keys can be specified as strings, URLs or local files.

- To specify a key directly, just provide the key as string.
- To load a key from file, prefix the path with the `file:` schema.
- To load a key from a URL, specify a URL with `http:` or `https:` schema.

```yaml
admins:
  - name: foo
    key: http://example.com/foo.pub
  - name: bar
    key: file:ssh-keys/bar.pub
  - name: baz
    key: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIF83wYwFxccj6boydYE5yoh+Tabuon7Uuu4HGlHrbpSt
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
      user_setup_delete_users: true
      admins:
        - name: foo
          key: http://example.com/foo.pub
        - name: bar
          key: file:ssh-keys/bar.pub
        - name: baz
          key: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIF83wYwFxccj6boydYE5yoh+Tabuon7Uuu4HGlHrbpSt
```


## Deleting Users

If `user_setup_delete_users` is set to `true` (default), the role will try to delete all users not in `admins`.
Users created via this role are part of the group `managed`.
The users being deleted are all users in the group `managed` which are not defined in `admins`.
Users with are not in the group `managed` will not be touched by this role.


## License

[BSD-3-Clause](LICENSE)
