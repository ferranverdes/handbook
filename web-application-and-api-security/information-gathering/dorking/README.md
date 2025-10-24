# Dorking

## Manual Google Dorks

* Find all pages of the domain:

    ```
    site:<domain>
    ```

* Find parameters:

    ```
    site:example.com inurl:"id"
    ```

* Find directory listing:

    ```
    site:example.com intext:"index of"
    ```

* Find login panel:

    ```
    site:example.com intitle:"admin" | intitle:"login"
    ```

    ```
    site:example.com intext:"admin" | intext:"login"
    ```

* Search for files with sensitive information:

    ```
    site:example.com filetype:(doc | pdf | xls | txt | ps | rtf | odt | sxw | psw | ppt | pps | xml)
    ```

## Automated Google Dorks

* [GooFuzz][3]

    ```bash
    ./GooFuzz -t example.com -e pdf,txt,doc,xml
    ```

    ```bash
    ./GooFuzz -t example.com -w files.txt
    ```

## Further reading

* [Google Hacking Database][1]
* [Google dork cheatsheet][2]

[1]: https://www.exploit-db.com/google-hacking-database
[2]: https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06
[3]: https://github.com/m3n0sd0n4ld/GooFuzz
