In the case where Consul has no leader you will need to preform a manual raft peers recovery.

Restoring from Peers Json
SSH into all the consul hosts and on one node execute:

```
consul operator raft list-peers -stale
```

This will give you a list of consul servers and their states:
```
Node                 ID                                    Address           State     Voter  RaftProtocol
i-006f96f15d330d331  c5ab6c9d-d363-91c4-560b-60ab64172d09  10.0.71.56:8300   follower  false   3
i-0a7bdf09286c03903  cd69366c-cc85-ba90-d344-ed304c73fdf7  10.0.76.163:8300  follower  false   3
i-0659d454cd54e6031  8834ac31-3282-fc89-e52d-c668fffbef02  10.0.86.216:8300  follower  false   3
```

You will need to construct a peers.json file in the following format using the values from the table above to force peers to join with each other:


```
[
  {
    "id": "c5ab6c9d-d363-91c4-560b-60ab64172d09",
    "address": "10.0.71.56:8300",
    "non_voter": false
  },
  {
    "id": "cd69366c-cc85-ba90-d344-ed304c73fdf7",
    "address": "10.0.76.163:8300",
    "non_voter": false
  },
  {
    "id": "8834ac31-3282-fc89-e52d-c668fffbef02",
    "address": "10.0.86.216:8300",
    "non_voter": false
  }
]
```
Once you’ve constructed this file you need to put it on disk in /opt/consul/data/raft/peers.json on each of the hosts.

Once that is done you can restart every node:
sudo systemctl restart consul
Consul will detect the file and join together and elect a leader.
