# Ansible Import Role

This is the Ansible MySQL role. It's designed for consumption by playbooks, not for consumption by itself. It pulls
down a specified copy of a database from an AWS S3 bucket and installs it into the MySQL database locally.

## Dependencies

This playbook is only tested using MySQL as set up by the role at:

- https://bitbucket.org/sitewards/ansible-role-mysql

## Limitations 

- This is not idempotent; each run of Ansible destroys the previous database, and creates the next one anew. It is
  difficult to determine if this is appropriate; the database is only in a "known" state when it is freshly imported.
- It is presumed that the object stored in S3 is an SQL file compressed with gzip, and stored in the format 
  `foo_bar.sql.gz`

## Compatibility

| OS           |
|--------------|
| Ubuntu 16.04 |

Untested on other platforms, but should work for 14.04 to 17.4

## Customisation

A limited number of options are configurable based on variables. For a full list and explanation of veriables, see the
defaults/main.yml file

## Usage

Include this in another ansible playbook. For sample, consider a generic server playbook:

```yaml
---
# $PLAYBOOK_ROOT/server.yaml
- name: "server"
  hosts: all
  become: true
  become_user: "root"
```

Add the reference for the role:

```yaml
# $PLAYBOOK_ROOT/server.yaml
# ...
become_user: "root"
roles
  - "import-db"
```

This will allow the role to be discovered. Then, add this repo as a submodule:

```bash
$ cd path/to/playbook/root
$ mkdir roles/
$ git submodule add git@bitbucket.org:sitewards/ansible-role-import-db.git roles/import-db
```

You should then check out the variables in `default/main.yml`, and set the variables that are required for your host in
`host_vars`. For further information about how default variables work, see

- http://docs.ansible.com/ansible/playbooks_variables.html

## Execution 

This playbook uses AWS keys supplied by the user locally to authenticate against S3. You will need to generate a set of
keys for your user, and export them into the execution environment for Ansible:

```
$ export AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY"
$ export AWS_SECRET_ACCESS_KEY="YOUR_SECRET_KEY"
```

Otherwise, the node cannot connect to the S3 bucket.
