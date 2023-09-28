# NTP in Tanzu and how the IP settings can propergate when a change is needed

In vCenter there are 2 places with in the supervisor cluster to hchange the NTP settings for the kubernetes clusters (supervisor/guest clusters) 

They are in the supervisor section under management and workload networks. 

![GitHub](sup1.png)

![GitHub](guest1.png)

The question is what happens when the management NTP IP is changed. 

Loggin onto the super visor node we see the current IP is 10.62.4.1

```
timedatectl show-timesync
LinkNTPServers=10.62.4.1
FallbackNTPServers=0.vmware.pool.ntp.org 1.vmware.pool.ntp.org 2.vmware.pool.ntp.org 3.vmware.pool.ntp.org
ServerName=10.62.4.1
ServerAddress=10.62.4.1
```

Now I am changing the IP in the vCenter GUI. 


After about ~45seconds the change is propagated to the supervisor nodes.

```
timedatectl show-timesync
LinkNTPServers=10.128.152.81
FallbackNTPServers=0.vmware.pool.ntp.org 1.vmware.pool.ntp.org 2.vmware.pool.ntp.org 3.vmware.pool.ntp.org
ServerName=10.128.152.81
ServerAddress=10.128.152.81
```





