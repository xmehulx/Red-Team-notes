Default database name is **ace** 

```Shell
$ mongo --port 27117 ace --eval "db.admin.find().forEach(printjson);"
```

> Use #mkpasswd to generate relevant password

```Shell
$ mongo --port 27117 ace --eval 'db.admin.update({"_id": ObjectId("Current_ID")},{$set:{"x_shadow":"mkpasswd_hash"}})'
```

