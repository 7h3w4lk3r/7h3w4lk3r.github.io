# ⭕ Physical Attacks

## <mark style="color:red;">Linux Physical Security Guide</mark>

{% embed url="https://www.secur.cc/physical-security-for-linux-systems" %}

## <mark style="color:red;">Boot in Single-user Mode</mark>

unauthorized users can edit the grub boot configuration and boot as root. After that the user can either collect any data in the users account(Passive). Or the unauthorized user can reset the root user password and/or other user passwords(Active).

### <mark style="color:orange;">Exploitation</mark>

when grub options are displayed press `‘e’`. Then you will land in a screen such as this. Now you must go to the kernal line as shown below and add in:

```
'init=bin/bash'
```

![](<../../.gitbook/assets/image (272) (1).png>)

Once that is done all you need to do is press f10 to boot. You may want to press fn + f10 in some cases. Then you will see a screen like this.

![](<../../.gitbook/assets/image (287) (1) (1).png>)

Now you must issue the commands:

```
mount -o remount,rw /
```

At this point, you are ready to reset the root password. So go ahead and change it. Finally, you need to issue the command `touch /.autorelabel`.(This is because you have done an unauthorized thing and need to clean the mess or clear the tracks).

![](<../../.gitbook/assets/image (296) (1) (1).png>)

Afterwards when you reboot into the machine, you will be able to access the root account with the new password you have used.

{% hint style="info" %}
#### The solution to this problem is to [enabling grub password authentication](../../hardening-and-monitoring/untitled/os-security/grub-hardening.md).
{% endhint %}

{% hint style="success" %}
#### Another (easy) option to gain a root shell in Linux a system is to simply boot up in rescue mode which is the second option in the GRUB boot menu and almost always available.&#x20;
{% endhint %}









