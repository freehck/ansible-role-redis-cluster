freehck.redis_cluster
=========

Deploys redis cluster (allows cross replication)

Description
-----------

This role enables to deploy a cross replicated redis cluster with the easy config.

Role Variables
--------------

The most important variables are listed here. For all the variables look into `defaults/main.yml`.

`redis_cluster_version`: version of the redis-server, default is `6.2.6`

`redis_cluster_install_dir`: redis will be installed here, default is `/opt/redis-{{ redis_cluster_version }}`

The only mandatory variable `redis_cluster_nodes`. The default is `[]`.

Example
-------

Inventory:

    [redis_all]
    redis01 ansible_host=10.1.1.1
    redis02 ansible_host=10.1.1.2
    redis03 ansible_host=10.1.1.3


Playbook:

    - hosts:
        - redis_all
      become: true
      vars:
        redis_cluster_nodes:

          # masters
          - name: master01
            host: redis01
            addr: "{{ hostvars['redis01'].ansible_host }}"
            port: 6378
            is_master: true

          - name: master02
            host: redis02
            addr: "{{ hostvars['redis02'].ansible_host }}"
            port: 6378
            is_master: true

          - name: master03
            host: redis03
            addr: "{{ hostvars['redis03'].ansible_host }}"
            port: 6378
            is_master: true

          # slaves
          - name: slave02
            host: redis01
            addr: "{{ hostvars['redis01'].ansible_host }}"
            port: 6379
            slave_of: master02

          - name: slave03
            host: redis02
            addr: "{{ hostvars['redis02'].ansible_host }}"
            port: 6379
            slave_of: master03

          - name: slave01
            host: redis03
            addr: "{{ hostvars['redis03'].ansible_host }}"
            port: 6379
            slave_of: master01
      
      roles:
        - name: freehck.redis_cluster



Install
-------

This role can be installed from [Ansible Galaxy](https://galaxy.ansible.com/):

`ansible-galaxy install freehck.redis_cluster`


License
-------

MIT

Author Information
------------------

Dmitrii Kashin, <freehck@freehck.com>
