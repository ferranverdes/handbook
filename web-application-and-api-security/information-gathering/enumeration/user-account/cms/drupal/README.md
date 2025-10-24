# Drupal

## Enumerate users

* It is possible to bruteforce usernames by default. If the user exists, the server will return 403 code, if not it will return 404:
  ```url
  https://example/user/<user>
  ```
* Accessing `/user/<number>` you can see the number of existing users. In this case that the webpage has 2 users, the request `/users/3` returns a not found error. 
* Try to create a username and if the name is already taken it will be notified:
  ```url
  https://example.com/user/register
  ```
* If you request a new password for a non-existing username, the web page will notify that the user is not recognized:
  ```url
  https://example.com/user/password
  ```

|Source: [*"Drupal" from Hacktricks*][1].|
|:--:|

[1]: https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/drupal
