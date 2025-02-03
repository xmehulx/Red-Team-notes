```shell-session
$ sudo neo4j start
```

# Enumerate ACLs
- Select starting node > `Node Info` > `Outbound Control Rights`
This option will show us objects we have control over directly, via group membership, and the number of objects that our user could lead to us controlling via ACL attack paths under `Transitive Object Control`

# Cypher Queries
## Users who can PSRemote
```cypher
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p2
```
### Tools
- [[Enter-PSSession]]
- [[Evil-WinRM]]
## SQL Admins
```cypher
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:SQLAdmin*1..]->(c:Computer) RETURN p2
```
### Tools
- [[MSSQLclient]]