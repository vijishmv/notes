## What is etcd
etcd is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed by a distributed system or cluster of machines. etcd is written in Go.
It uses Raft concensus alogrithm to comminucate with nodes and leader election.
etcd is used by kubernetes, project calico, rook, coreDNS etc.

## Alternatives to etcd
Apache ZooKeeper
HashiCorp Consul
Redis

## Versions
etcd has 2 major versions still in use: v2 and v3

## Differences between etcd v2 and etcd v3

|etcd v2|etcd v3|
|:------|:------|
Based on HTTP|Based on gRPC (uses HTTP/2 and ProtoBuf)
Holds key values in a hierarchical tree|Holds key values in a simple flat layout
Only supports v2 API commands|Supports both v2 API commands as well as v3 API commands

Though etcd v3 supports v2 api commands. The data that is created through v2 is only available through v2 commands and data created through v3 is available through v3 commands.

## Ports used by etcd
**2379** for client requests  
**2380** for peer communication

## etcd v3 enviroment varables
The following can be exported to achieve the same functionality as their commandline option using v3 api
```
ETCDCTL_API=3
ETCDCTL_CACERT=/etc/etcd/ca.crt         # --trusted-ca-file=/etc/etcd/ca.crt
ETCDCTL_CERT=/etc/etcd/etcd-server.crt  # --cert-file=/etc/etcd/etcd-server.crt
ETCDCTL_KEY=/etc/etcd/etcd-server.key   # --key-file=/etc/etcd/etcd-server.key
# --endpoints
```

## Basic etcd v3 commands

```
root@master-1:~# etcdctl put key1 value1
OK
root@master-1:~#

root@master-1:~# etcdctl get key1
key1
value1
root@master-1:~#

etcdctl snapshot save /tmp/etcd_snapshot.db
etcdctl snapshot status /tmp/etcd_snapshot.db

etcdctl snapshot restore snapshot.db \
  --data-dir=/var/lib/etcd-from-backup \
  --initial-cluster master-1=https://192.168.5.11:2380,master-2=https://192.168.5.12:2380 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-advertise-peer-urls https://${INTERNAL_IP}:2380
```

