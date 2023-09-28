# NTP in Tanzu and how the IP settings can propergate when a change is needed

In vCenter there are 2 places with in the supervisor cluster to hchange the NTP settings for the kubernetes clusters (supervisor/guest clusters) 

They are in the supervisor section under management and workload networks. 

![GitHub](sup1.png)

![GitHub](guest1.png)

The question is what happens when the management NTP IP is changed. 



