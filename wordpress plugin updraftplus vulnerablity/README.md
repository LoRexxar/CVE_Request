# wordpress plugin updraftplus vulnerablity

This erroneous report has been withdrawn. In both cases, the alleged "attack" could only be carried out by a user who had the ability to log in as a WordPress administrator.

In admin.php in plupload_action():

```
if (!UpdraftPlus_Options::user_can_manage()) exit;
```

In admin.php in updraft_ajax_handler():

```
if (if (!UpdraftPlus_Options::user_can_manage()) return;
```
