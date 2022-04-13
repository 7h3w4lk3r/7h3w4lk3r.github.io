# ⭕ Privilege Escalation

## <mark style="color:red;">know before you start</mark>

**1. first rule of privilege escalation is that you HAVE TO be familiar with target system command line, actually the more comfortable you are with the shell the better and easier will be the privesc phase.**

**2. DO NOT jump to kernel exploits FIRST. yes, yes, in a CTF its a low-hanging fruit but in a real life scenario you dont want to crash the system SPECIALLY web and file servers, do you?**

**3. whatever tool you use for prives or whats called the post-exploitation information gathering, remember you should use manual techniques too. make sure you didnt miss anything.**

**4. all privilege escalations are effectively examples of access control violations. so if you can access what you want, there is usually no need to gain a root shell, except in a CTF or when you're trying to impress your client.**

## <mark style="color:red;">Resources</mark>

{% embed url="https://book.hacktricks.xyz/linux-unix/privilege-escalation#apparmor" %}

{% embed url="https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/" %}

{% embed url="https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md" %}

{% embed url="https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_-_linux.html" %}

{% embed url="https://resources.infosecinstitute.com/topic/privilege-escalation-linux-live-examples/" %}

{% embed url="https://hackmag.com/security/linux-privileges-escalation" %}

## <mark style="color:red;">Automated Tools</mark>

a list of automated tools used in post-exploitation enumeration and privilege escalation.

{% embed url="https://github.com/AlessandroZ/BeRoot" %}

{% embed url="https://github.com/rebootuser/LinEnum" %}

{% embed url="https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS" %}

{% embed url="https://github.com/sleventyeleven/linuxprivchecker/blob/master/linuxprivchecker.py" %}

{% embed url="https://github.com/WazeHell/PE-Linux" %}

{% embed url="https://github.com/diego-treitos/linux-smart-enumeration" %}

{% embed url="https://github.com/mzet-/linux-exploit-suggester" %}

{% embed url="https://github.com/pentestmonkey/unix-privesc-check" %}
