To get Lauzet CLI tools to work with burp there two things need to be solved.
# Proxychains on M1
![[root.tools.proxychains#M1 problem solution]]

# Lipki cert pinning
Lipki tools use their own certificate pinning.
1. Prepare Burp certificate in PEM format
2. Make a backup copy of:
	1. `/etc/lipki/ca-bundle.crt`
	2. `/etc/lipki/ca-bundle.checksum`
3. Append burp cert to the ca-bundle:
   ```
   sudo bash -c "cat burp.pem >> /etc/lipki/ca-bundle.crt"
   ```
4. Calculate the new checksum:
```
cat /etc/lipki/ca-bundle.crt | sha256
```
5. Put the new checksum into `/etc/lipki/ca-bundle.checksum`:
   ```json
{
"ca-bundle.crt": {"sha256": "new-sha"},
"ca-bundle.metadata.json": {"sha256": "leave as is"}
}
```
