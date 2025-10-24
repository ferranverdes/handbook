# File and Directory Enumeration

## Enumerate directories

* `robots.txt` file.

### [feroxbuster][2]

```bash
feroxbuster -u https://example.com -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -C 403 -d 0 -o dir-enum.feroxbuster
```

Options to include:

  * `-a <user-agent>`.

## Enumerate files

### feroxbuster

#### On directories found previously

```bash
awk -F' ' '{print $5}' dir-enum.feroxbuster | feroxbuster --stdin -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt -C 301,302,403 -o file-enum.feroxbuster
  ```

```bash
awk -F' ' '{print $5}' dir-enum.feroxbuster | feroxbuster --stdin -w /usr/share/seclists/Discovery/Web-Content/common.txt -C 301,302,403 -x bak,conf,old -o file-enum.feroxbuster
```

*Important note: ensure the root directory is not omitted.*

#### Recursively as directories are found

```bash
feroxbuster -u https://example.com -w /usr/share/seclists/Discovery/Web-Content/common.txt -C 403 -d 0 -x bak,conf,old -o file-enum.feroxbuster
```

In this case, it uses the same wordlist to find directories and files.

## Enumerate directories and files

### [dirsearch][3]

```bash
./dirsearch.py -u http://example.com -e php -x403
```

## Common dictionaries

* [SecLists web content discovery][1] available on `/usr/share/seclists/Discovery/Web-Content/` once installed:
  * `raft-medium-directories.txt`
  * `directory-list-2.3-medium.txt`
  * `common.txt`
* `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` on Kali Linux.
* `/usr/share/dirb/wordlists/common.txt` on Kali Linux.

## List of backup file extension

* `.bak`
* `.bac`
* `.old`
* `000`
* `~`
* `01`
* `001`
* `_bak`
* `.inc`
* `.xxx`

[1]: https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content
[2]: https://github.com/epi052/feroxbuster
[3]: https://github.com/maurosoria/dirsearch
