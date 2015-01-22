# DHCP Role

[![Build
Status](https://travis-ci.org/dbryant4/ansible-role-dhcp.svg?branch=master)](https://travis-ci.org/dbryant4/ansible-role-dhcp)

## Description

This role manages the installation of DHCPd on a Raspberry Pi, although it
should be able to manage any other Debian based system. When paired with
the [bind role](https://github.com/dbryant4/ansible-role-bind), this
role will set a DHCP server which will automatically create A and PTR
records for new hosts whcih recieve an IP address from this DHCP server.

## Provides

1. An DHCP server

## Requires

1. Ansible 1.6 or higher
2. Raspberry Pi (possibly other Debian based systems)

## Variables

- dhcp_static_hosts - A hash defining static IP address assignments. `name`, `mac`, and `ip` are required for each static host.
  - name - the hostname of the client
  - mac - the mac address of the client using colons
  - ip - the IP you want to assign the client.
- dhcp_ddns_server - The DNS server where the DHCP server will report new clients. Use `localhost` if using the [bind role](https://github.com/dbryant4/ansible-role-bind).
- dhcp_name_server - The IP of the DNS server clients should use.
- dhcp_domain - The local domain to use. This is used to provide a
  search domain to clients.
- dhcp_subnet_mask: The subnet mask to use for the DHCP network. Default '255.255.255.0'
- dhcp_range - This defines the range of IPs which will be assigned to
  clients. The pool of addresses will range from `low` to `high`.
  - low - the first IP to assign to requesting clients
  - high - the last IP to assign to requesting clients
- dhcp_network_ip - The IP representing the network. Ususally it's the
  first IP in the subnet. Example: '192.168.101.0'
- dhcp_router: The IP of the default route to pass on to clients.
- dhcp_broadcast_ip: The broadcast IP for the DHCP network. Usually this is the last IP in the subnet. Example: '192.168.101.255'
- dhcp_reverse_zone_name: The reverse zone name. This is used to report
  to the DNS server when creating a new PTR record. Example: '101.168.192'
- dhcp_service_name - The service name of the DHCPd service. This should
  not be chaned. Default: 'isc-dhcp-server'

### Changing Variable Values

To Change the value of variables, create a file in `host_vars/` or `group_vars/` or define variables in the playbook.

There are other options for changing variable values. See [Ansible
Variable
Documentation](http://docs.ansible.com/playbooks_variables.html) for
more ideas.

## Usage

Include this role in your plays and set varaibles as desired.

```yaml
---
  name: tftp-servers
  hosts: tftp-servers
  vars:
    tftp_centos_mirror: 'http://mymirror.local/centos/7/x86_64'
    tftp_ks_method:     'http://mymirror.local/centos/7/x86_64'
  roles:
    - tftp
```

## Tests
This role includes Travis CI tests.
