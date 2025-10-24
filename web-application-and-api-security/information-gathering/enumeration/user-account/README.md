# User Account Enumeration

## User enumeration via error messages

The following are just some examples:

  * *"Login for user foo; invalid password."*
  * *"Login failed, invalid user ID."*
  * *"Login failed; account disabled."*

### [wfuzz][3]

```bash
wfuzz -c -u http://example.com/login.php -d "username=FUZZ&password=hello&action=login" -w usernames.txt --hw 3418
```

### Patator

```bash
patator http_fuzz url=http://example.com/login.php method=POST body='username=FILE0&password=test' 0=/root/Desktop/users.lst follow=1 accept_cookie=1 -x ignore:fgrep='Invalid Username'
```

### [Burp Suite][2]

## User enumeration via website behavior

The following are just some examples:

  * When the user does not exist cookies are deleted. Instead, when user exists cookies are not deleted or a new cookie is set.
  * When the user does not exist it redirects to a known fixed page. Instead, when user exists it redirects to a user-specific page.
  * When the user does not exist the HTML is fixed. Instead, when user exists the HTML changes or is different from an invalid user.

### Different value in a JavaScript variable

For example, a specific JavaScript variable has a value of "1" instead of "0" when a user exists.

#### [wfuzz][3]

```bash
wfuzz -c -u http://example.com/login.php -d "username=FUZZ&password=hello&action=login" -w usernames.txt --ss "var userAuth = 1;"
```

### Cookie setted

For example, a new cookie is set with the value "wrong user" by the application if the user does not exist.

#### Patator

```bash
patator http_fuzz url=http://example.com/login.php method=POST body='username=FILE0&password=test' 0=/root/Desktop/users.lst follow=1 accept_cookie=1 -x ignore:fgrep='wrong_user'
```

## User enumeration via timing attacks

  * User does not exist in the DB: show error and abort.
  * User exists in the DB: retrieve user, calculate password, check if it matches.

Because of this, it may be possible to perform user enumeration by timing how long it takes to the application to send a response.

## Common dictionaries

* `/usr/share/nmap/nselib/data/usernames.lst` on Kali Linux.

[2]: /burp-suite/intruder/
[3]: https://www.kali.org/tools/wfuzz/
