## API calls

### Indices:
```Shell
curl https://ELASTIC_IP:ELASTIC_PORT/_cat/indices -u 'USER:PASS'
curl -s -q -x socks5://127.0.0.1:1080 -k https://127.0.0.1:9200/_cat/indices -u 'user:DumpPassword$Here'
```

### Seed:
```Shell
curl https://ELASTIC_IP:ELASTIC_PORT/SEED/_search/ -u 'USER:PASS'
```
