# WordPress

## WPScan

### Customize User-Agent header

```bash
wpscan --url http://example.com --user-agent <value>
```

> Using something common, like a well-known web browser user-agent will help with evading some basic defenses.

### Default and non-intrusive scan

```bash
wpscan --url http://example.com
```

### Enumerate plugins

```bash
wpscan --url http://example.com --enumerate p
```

## Plecost

* [Plecost][1] is another excellent tool for enumerating plugins.

### Enumerate plugins

```bash
plecost -i /usr/share/plecost/wp_plugin_list.txt http://example.com
```

[1]: https://github.com/iniqua/plecost
