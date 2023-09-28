# NTP in Tanzu and how the IP settings can propergate when a change is needed

In vCenter there are 2 places with in the supervisor cluster to hchange the NTP settings for the kubernetes clusters (supervisor/guest clusters) 

They are in the supervisor section under management and workload networks. 

![GitHub](sup1.png)

![GitHub](guest1.png)

# The question is what happens when the management NTP IP is changed? 

Loggin onto the super visor node we see the current IP is 10.62.4.1

```
timedatectl show-timesync
LinkNTPServers=10.62.4.1
FallbackNTPServers=0.vmware.pool.ntp.org 1.vmware.pool.ntp.org 2.vmware.pool.ntp.org 3.vmware.pool.ntp.org
ServerName=10.62.4.1
ServerAddress=10.62.4.1
```

Now I am changing the IP in the vCenter GUI. 

![GitHub](mgtchange1.png)


After about ~45seconds the change is propagated to the supervisor nodes.

```
timedatectl show-timesync
LinkNTPServers=10.128.152.81
FallbackNTPServers=0.vmware.pool.ntp.org 1.vmware.pool.ntp.org 2.vmware.pool.ntp.org 3.vmware.pool.ntp.org
ServerName=10.128.152.81
ServerAddress=10.128.152.81
```

# The next question is what happens when the workload NTP IP is changed?

I have currenly 2 clusters ( I am on the supervisor controll plane): 

```
root@4223a1b0a09ff5f75249d31b10f724a1 [ ~ ]# k get clusters -A
NAMESPACE       NAME              PHASE         AGE   VERSION
namespace1000   miniocluster      Provisioned   21h   v1.23.8+vmware.2
namespace1000   tmclocalcluster   Provisioned   77d   v1.24.9+vmware.1
```

Looking at the nodes: 

```
root@4223a1b0a09ff5f75249d31b10f724a1 [ ~ ]# kubectl get virtualmachines -A
NAMESPACE       NAME                                                POWER-STATE   AGE
namespace1000   miniocluster-l95rz-5b729                            poweredOn     21h
namespace1000   miniocluster-node-pool-1-xblpt-64f896dcb-8sdkm      poweredOn     21h
namespace1000   tmclocalcluster-db9s7-l4r2n                         poweredOn     21h
namespace1000   tmclocalcluster-node-pool-1-w28f7-99556fbd9-759hj   poweredOn     21h
namespace1000   tmclocalcluster-node-pool-1-w28f7-99556fbd9-wc449   poweredOn     21h
namespace1000   tmclocalcluster-node-pool-1-w28f7-99556fbd9-wmtx6   poweredOn     21h
```









