# Appendix

**Create and distribute ssh keys**

Our lab has already populated the hosts with passwordless ssh keys. These keys can be created easily and distributed to hosts using the `ssh-copy-id` command:
```
[student@workstation ~]$ sudo ssh-keygen -b 2048 -t rsa -f /root/.ssh/id_rsa -q -N ""
[student@workstation ~]$ for i in master node1 node2; do ssh-copy-id root@$i; done
```

**Contributors**
```
Mark Seida
Christoph Doerbeck
Gordon Keegan
Justin Pittman
```
**Testing, formatting, and support**
```
Gareth Jenkins
Banu Bhandaru
Matthew Ward
Yvonne Herman
Matt St. Onge
Brian Tannous

Many others. 
```
