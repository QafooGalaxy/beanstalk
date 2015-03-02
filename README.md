Qafoo Beanstalk
===============

Ansible Role for Beanstalk Message Queue.

Requirements
------------

- Ubuntu Server
- Servers with this role must have the Ansible group `beanstalk`

Resource
--------

Beanstalk provides a resource for applications. It registers
a service on domain `beanstalk.service.consul` and provides
configuration through Consul:

    $ curl http://localhost:8500/v1/kv/apps/_global/beanstalk/connection

And environment variable:

    $ export BEANSTALK_CONNECTION="beanstalk.service.consul:11300"

Variables
---------

Beanstalk Default Configuration

    ---
    beanstalk_listen: "{{ ansible_all_ipv4_addresses|ipaddr('private')|first|default(['127.0.0.1']) }}"
    beanstalk_port: 11300
    beanstalk_extra: ""
    beanstalk_consul_service: "beanstalk"
    beanstalk_apps: ['_global']

Uses the first private ip-address of the host that is designated as beanstalk machine.

Example Playbook
----------------

Minimal example to setup:

    ---
    - hosts: beanstalk
      tasks:
        - role: beanstalk
          beanstalk_apps: ['www']

With the `beanstalkd_extra` option you can set all the necessary variables, for
example persistence and job size, allowing to register multiple beanstalk
servers with consul:

    ---
    - hosts: beanstalk1
      roles:
        - role: beanstalk
          beanstalk_port: 11500
          beanstalk_extra: "-z 100000 -b /var/lib/beanstalk"
          beanstalk_consul_service: "beanstalk1"
          beanstalk_apps: ['foo']
    - hosts: beanstalk2
        - role: beanstalk
          beanstalk_port: 11400
          beanstalk_extra: "-z 100000 -b /var/lib/beanstalk"
          beanstalk_consul_service: "beanstalk2"
          beanstalk_apps: ['bar']

License
-------

BSD License.

Author Information
------------------

Benjamin Eberlei <benjamin@qafoo.com>
https://github.com/QafooGalaxy
