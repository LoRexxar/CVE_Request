GetSimpleCMS 3.3.15 mul vuls



# any url redirection in function redirect

in function `redirect`，we will redirect to parameter `$url` without any check

```
function redirect($url) {
	global $i18n;

	// handle expired sessions for ajax requests
	if(requestIsAjax() && !cookie_check()){
		header('HTTP/1.1 401 Unauthorized', true, 401);
		header('WWW-Authenticate: FormBased');
		die();
	}	

	if (!headers_sent($filename, $linenum)) {
    header('Location: '.$url);
  } else {
    echo "<html><head><title>".i18n_r('REDIRECT')."</title></head><body>";
    if ( !isDebug() ) {
	    echo '<script type="text/javascript">';
	    echo 'window.location.href="'.$url.'";';
	    echo '</script>';
	    echo '<noscript>';
	    echo '<meta http-equiv="refresh" content="0;url='.$url.'" />';
	    echo '</noscript>';
	  }
    echo i18n_r('ERROR').": Headers already sent in ".$filename." on line ".$linenum."\n";
    printf(i18n_r('REDIRECT_MSG'), $url);
		echo "</body></html>";
	}
	
	exit;
}
```

if we can control parameter `$url`, we can lead any url redirection.

just like line 206 in `/admin/changedata.php`，parameter `$redirect_url` input from `$_POST['redirectto']` without any check.

![image.png-210.1kB][1]

so if we set `$redirect_url` and we can redirect to any url.

![image.png-146.6kB][2]


# Limited Reflective xss in function redirect

in function `redirect`, if we can control the part of parameter `$redirect_url` and function `headers_sent` return True. the parameter `$url` will be spliced into javascript script. 

we can use double quote to escape and execute any javascript script.

![image.png-218.9kB][3]

if we can control parameter `$url`, we can lead Reflective xss.

just like line 206 in `/admin/changedata.php`，parameter `$redirect_url` input from `$_POST['redirectto']` without any check.


![image.png-26.5kB][4]

# Reflective xss in /admin/settings.php

in `/admin/settings.php` we can set website setting for GetSempleCMS, the parameter `$TIMEZONE` will be set from `$_POST` without any filter.

![image.png-123.9kB][5]

we can use double quote to escape and execute any javascript script.

![image.png-77.9kB][6]

# Reflective xss in /admin/setup.php

in `/admin/setup.php` we should input sitename、username、email to setup website. but if any error in the installation, these three parameters will be returned back to the page without any filter.

![image.png-244.5kB][7]

we can use double quote to escape and execute any javascript script.

![image.png-59.5kB][8]



  [1]: http://static.zybuluo.com/LoRexxar/4ocdmk0q8qgwosiv89ag4e25/image.png
  [2]: http://static.zybuluo.com/LoRexxar/r8njsdlaa4w9djc4uz3aftzn/image.png
  [3]: http://static.zybuluo.com/LoRexxar/3dpzghi8doooio069mca1go2/image.png
  [4]: http://static.zybuluo.com/LoRexxar/0acmcqx4delxblv5cahmtqmg/image.png
  [5]: http://static.zybuluo.com/LoRexxar/xbvfsuv0kod1jnmle7v9q3k3/image.png
  [6]: http://static.zybuluo.com/LoRexxar/5wne34e4vllp91zd7t5z7unx/image.png
  [7]: http://static.zybuluo.com/LoRexxar/nsrco2onw1avldvil1fwiv1s/image.png
  [8]: http://static.zybuluo.com/LoRexxar/y0h9cbqf96dqqjf8lgb4ifmt/image.png