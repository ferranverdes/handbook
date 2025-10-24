# Resource Enumeration

## Find hidden resources

```
wfuzz -v --hc 404 -z range,1-10000 http://example.com/index.php?id=FUZZ
```
