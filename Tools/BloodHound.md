```shell-session
$ sudo neo4j start
```

# Enumerate ACLs
- Select starting node > `Node Info` > `Outbound Control Rights`
This option will show us objects we have control over directly, via group membership, and the number of objects that our user could lead to us controlling via ACL attack paths under `Transitive Object Control`.