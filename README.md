# DDNS Ansible Playbook
Ansible playbook to orchestrate the deployment of a Docker-based Dynamic DNS solution, built on top of two separeted containers: a [ISC DHCP container](http://hub.docker.com/r/networkboot/dhcpd) and a [BIND container](http://hub.docker.com/r/plgr/bind).
Both containers may be hosted by the same or differente hosts.

## How to use it
1. Either configure the existing or create your own <code>hosts</code> file following the example:

  ```
  [dhcp]
  dhcp.lan.example.com

  [dns]
  dns.lan.example.com
  ```
  Note that the servers must run Ubuntu Server x86_64 12.04 or newer.

2. Configure the variables inside the following files:

  * group_vars/all;

  * group_vars/dhcp;

  * group_vars/dns.

3. Generate a shared key to your DDNS deployment (you will need bind9utils installed in order to do that):

  ```bash
  dnssec-keygen -a HMAC-MD5 -b 128 -r /dev/urandom -n USER DDNS_UPDATE
  grep DDNS_UPDATE Kddns_update.*.key | cut -d' ' -f7
  ```

4. Now, store the key inside a environment variable called DDNS_SECRET, and run the playbook as usual:

  ```bash
  DDNS_SECRET=<generated key> ansible-playbook -i <path to hosts file> --ask-become-pass site.yml
  ```

## What else?
Maintained by [Lucas Vieira Souza da Silva](lucas@vieira.io). August 2016.

Licensed under Apache 2.0 license. Check the LICENSE file.

If will find any problem and if you think that any thing could be improved, please, report an issue and write a PR! Thank you very much :)
