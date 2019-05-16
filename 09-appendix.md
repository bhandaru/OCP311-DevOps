# Appendix

**Create and distribute ssh keys**

Our lab has already populated the hosts with passwordless ssh keys. These keys can be created easily and distributed to hosts using the `ssh-copy-id` command:
```
[student@workstation ~]$ sudo ssh-keygen -b 2048 -t rsa -f /root/.ssh/id_rsa -q -N ""
[student@workstation ~]$ for i in master node1 node2; do ssh-copy-id root@$i; done
```

**Free up the /dev/vdc disk used by gluster**
Execute the following from workstation as root
```
for i in node1 node2 node3
do
echo $i
sudo ssh $i "lvm vgremove -f \$(vgs | grep vg_ | awk '{print $1}')"
sudo ssh $i "wipefs -a /dev/vdc"
done
```

**Contributors**
```
Mark Seida
Christoph Doerbeck
Gordon Keegan
Justin Pittman
Banu Bhandaru
```
**Testing, formatting, and support**
```
Gareth Jenkins
Matthew Ward
Yvonne Herman
Matt St. Onge
Brian Tannous

Many others. 
```
