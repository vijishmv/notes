## How to determine the IP address and routes of a pod without attaching to it

We are going to determine the IP addresses and routes of a pod 'alpinepod' without running 'kubectl exec -it aplinepod -- ip address show'

### Determine the worker where the pod runs
```
root@master-1:~# kubectl get pod alpinepod  -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP          NODE       NOMINATED NODE   READINESS GATES
alpinepod   1/1     Running   2          13d   10.32.0.3   worker-2   <none>           <none>
root@master-1:~#
```
### SSH into the relevant worker

### Determine the container IDs
```
root@worker-2:~# docker ps -f "name=alpinepod"
CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS              PORTS               NAMES
7ca131241f54        alpine                 "sleep 3600"        34 minutes ago      Up 34 minutes                           k8s_alpinepod_alpinepod_default_62bbb06b-e631-11ea-b834-0228762fc2b7_5
cc50bccd4582        k8s.gcr.io/pause:3.1   "/pause"            4 hours ago         Up 4 hours                              k8s_POD_alpinepod_default_62bbb06b-e631-11ea-b834-0228762fc2b7_2
root@worker-2:~#
```
### Determine the PID of the pause container 
```
root@worker-2:~# docker inspect -f '{{.State.Pid}}' cc50bccd4582
3228
root@worker-2:~#
```
### Determine the different namespaces of this PID
```
root@worker-2:~# lsns | grep 3228
4026532280 mnt         1  3228 root            /pause
4026532281 uts         1  3228 root            /pause
4026532282 ipc         2  3228 root            /pause
4026532283 pid         1  3228 root            /pause
4026532285 net         2  3228 root            /pause
root@worker-2:~#
```
### Create the network namespace directory in /var/run
```
root@worker-2:~# ln -sf /proc/3228/ns/net /var/run/netns/4026532285
root@worker-2:~#
```

### Determine the ipaddress and routes from the network namespace using ip 
```
root@worker-2:~# ip netns exec 4026532285 ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
14: eth0@if15: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1376 qdisc noqueue state UP group default
    link/ether 5a:c4:33:a5:af:cf brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.32.0.3/12 brd 10.47.255.255 scope global eth0
       valid_lft forever preferred_lft forever
root@worker-2:~#

root@worker-2:~# ip netns exec 4026532285 ip route list
default via 10.32.0.1 dev eth0
10.32.0.0/12 dev eth0 proto kernel scope link src 10.32.0.3
root@worker-2:~#
``` 
