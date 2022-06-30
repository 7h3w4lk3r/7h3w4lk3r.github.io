# Grub Hardening

GRUB is the easiest way to hack into Linux systems !

1. First option to break into a Linux system is to edit the line **`ro quite splash`** as **`rw init=/bin/bash`** during system startup (usually triggered by pressing “e” on the list).
2. Second option would be using the **Recovery Mode**.

## <mark style="color:red;">Debian-based</mark>

### <mark style="color:orange;">Disable Recovery</mark>

```
root@debian:~# nano /etc/default/grub
....
# Uncomment to disable generation of recovery mode menu entries
GRUB_DISABLE_RECOVERY="true"
....
root@debian:~# update-grub
```

### <mark style="color:orange;">Password Protection</mark>

Generate a password for grub:

```
grub-mkpasswd-pbkdf2
```

You will be prompted to create and verify a password for GRUB

Once that completes, the command will generate a hashed password. The hash will begin with grub and end with a long string of characters. You'll need to copy that down.

```
sudo nano /etc/grub.d/00_header
```

At the bottom of that file, paste the following:

```
cat << EOF
set superusers="admin"
password_pbkdf2 admin HASH
EOF
```

where HASH is the hash generated earlier.

Save and close that file. Update GRUB with the command:

```
sudo update-grub
```

Reboot your system. Soon after the splash screen appears, you should be prompted to type a username. In the configuration above we created the admin user, which will require the password we added with the `grub-mkpasswd-pbkdf2` command. Once you type the username, hit Enter, and you'll be prompted for the password.

Once you've successfully typed the password, you'll either find yourself in Single User mode (if you opted for that boot method) or at the login prompt.

## <mark style="color:red;">Redhat 7-based</mark>

generate a password

```
grub2-mkpasswd-pbkdf2
```

Type the password which you want. It will generate the hash of your password which you can see in the below picture, I have covered it with the green line. You have to copy it.

```
nano /etc/grub.d/10_linux
```

After opening this file go to the end of the file, and type below lines as it is and change the user name which you want to give and make sure that user has been created in this server system and put the password at the place after username then replace the \[HASH] with the hash you got.

```
cat << EOF
set superusers="root"
password_pbkdf2 root [HASH]
EOF
```

Save the file

## <mark style="color:red;">Redhat 8-based</mark>

```
grub2-setpassword
```

This command generates a hashed password that is stored in the /boot/grub2/user.cfg file.

By default user.cfg does not exist. If you have previously generated a password using the command grub2-setpassword then this file will exist, otherwise this command will generate a new file.

You can display the generated password using the cat command.

```
cat /boot/grub2/user.cfg 
```

Recreate the GRUB2 Configuration file

```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

We have successfully set the grub2 password
