# Ansible Role: Git Server

[![Build Status](https://travis-ci.com/daveol/ansible-role-git_server.svg?branch=master)](https://travis-ci.com/daveol/ansible-role-git_server)

Setup a simple git remote repository using ssh, this does not have access control features, everyone that has access can pull and push to all repostories. If you need an access control, use [gitolite](https://gitolite.com/gitolite/index.html) or something similair.

## Requirements

A host that has ssh installed and configured, it will not adjust any of the ssh host configuration.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):
```yaml
git_keys: []
```
All public ssh keys that will get access to all the git repositories.
```yaml
git_repos:
  - path: test.git
    allow_force: false
```
A list of git repositories to generate:
 - `path` is the path on the server relative to `git_dir`
 - `allow_force: true` allows force push to the repository and is optional.
```yaml
git_user: git
```  
Username to use for login, defaults to git.
```yaml
git_user_password: '!'
```
User password for git if you want to enable this use the `password_hash` filter to generate a hashed (and salted) password. A `!` in password disables password login.
```yaml
git_dir: /var/lib/git
```
The location to store the `git_repos`, also the user directory for `git_user`.
```yaml
git_shell: /usr/bin/git-shell
```
The shell used for the `git_user` leaving it to the default (`/usr/bin/git-shell`) will limit the user to only `git-receive-pack`, `git-upload-pack`, `git-upload-archive` and the commands in `{{ git_dir }}/git-shell-commands`. This role automatically installs a `no-interactive-login` script for [git-shell](https://git-scm.com/docs/git-shell), and can install a `cvs` command for compatibilty with cvs clients.
```yaml
git_enablerepo: ""
```
This variable, a well as `git_packages`, will be used to install git via a particular `yum` repo (RedHat systems only). Any additional repositories you have installed that you would like to use for a newer/different Git version.
```yaml
git_packages:
  - git
```
The specific Git packages that will be installed. By default, only `git` is installed, but you could add additional git-related packages like `git-cvs` if desired.

## Dependencies

None.

## Example Playbook

    - hosts: servers
      vars:
        git_keys:
          - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFhyob72VL/7+5Jocjoqb9KtWwiUy6DM5/AN/jZjXZeL dave@bewaar.me
        git_repos:
          - path: test.git
      roles:
        - { role: daveol.git_server }

## License

MIT / BSD
