Multi-instance Redis
====================

This is an Ansible role for provisioning multiple [Redis](https://redis.io/) instances, with customisable configurations for each one.

Defaults
--------

Check out this role's overridable defaults [here.](defaults/main.yml) 


Setup
-----

There's a few tunable defaults, but the main variable is `multiredis_instances`. 
An example for setting up 3 Redis instances for different parts of an app might look like this:
```
multiredis_instances:
  - name: cache
    port: 6380
    configs: |
      maxmemory 150mb
      maxmemory-policy allkeys-lru

  - name: jobs
    port: 6381
    configs: |
      maxmemory 50mb
      maxmemory-policy noeviction
      appendonly yes
      appendfsync everysec

  - name: sessions
    port: 6382
    configs: |
      maxmemory 100mb
      maxmemory-policy volatile-lru
```

Check out the Redis docs [here](https://redis.io/topics/lru-cache) for more info on specific configuration options.

Example playbook
----------------

```
- name: Multi Redis
  hosts: webservers

  roles:
    - role: libre_ops.mutli_redis
```
