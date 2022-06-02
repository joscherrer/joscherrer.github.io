
## Sample GET request

```python
import urllib.request

f = urllib.request.urlopen('https://www.google.fr')
print(f.read().decode(f.info().get_content_charset()))
```

## Sample POST request

```py
from urllib.parse import urlencode
from urllib.request import Request, urlopen

url = "https://www.google.fr"
data = urlencode({'test1': 1}).encode()

req = Request(url, data=data)
res = urlopen(req)

print(res.read().decode(res.info().get_content_charset()))
```
