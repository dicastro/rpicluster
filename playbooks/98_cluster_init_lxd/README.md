This playbook has been highly simplified. See the [source](https://github.com/juju4/ansible-lxd) to get a more flexible and generic version of the playbook.

To run the playbook:

```
ANSIBLE_CONFIG=ansible.cfg ansible-playbook ./playbooks/all.yml
```


# Doc

https://blog.simos.info/how-to-make-your-lxd-container-get-ip-addresses-from-your-lan
https://blog.simos.info/how-to.preconfigure-lxd-containers-with-cloud-init
    - ejemplo de networkconfig con cloud-init y lxd
      - https://linuxcontainers.org/lxd/docs/master/cloud-init

https://blog.simos.info/how-to-make-your-lxd-containers-get-ip-addresses-from-your-lan-using-a-bridge

https://www.ubuntupit.com/how-to-configure-and-use-network-bridge-in-ubuntu-linux