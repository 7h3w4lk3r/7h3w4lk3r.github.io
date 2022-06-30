# Password Security & Sudoers

{% embed url="https://www.tecmint.com/sudoers-configurations-for-setting-sudo-in-linux" %}

{% hint style="info" %}
Any changes made to `/etc/login.defs` will only be applied if the `usermod` command is used. If user IDs are added a different way, use the `chage` command to effect changes to individual user IDs.
{% endhint %}

## <mark style="color:red;">Login Security</mark>

### <mark style="color:orange;">Restrict su Command</mark>

The su command allows a user to run a command or shell as another user. The program has been superseded by sudo , which allows for more granular control over privileged access. Normally, the su command can be executed by any user. By uncommenting the pam\_wheel.so statement in /etc/pam.d/su , the su command will only allow users in the wheel group to execute su .

Add the following line to the `/etc/pam.d/su` file:

```
auth required pam_wheel.so use_uid
```

Create a comma separated list of users in the wheel statement in the /etc/group file:

```
wheel:x:10:root,<user list>
```

## <mark style="color:red;">Password Security</mark>

### <mark style="color:orange;">Expiration</mark>

Set the PASS\_MAX\_DAYS parameter to conform to site policy in `/etc/login.defs` :

```
PASS_MAX_DAYS 365
```

Modify user parameters for all users with a password set to match:

```
chage --maxdays 365 <user>
```

You can also check this setting in `/etc/shadow` directly. The 5th field should be 365 or less for all users with a password.

{% hint style="info" %}
A value of -1 will disable password expiration. Additionally the password expiration must be greater than the minimum days between password changes or users will be unable to change their password.
{% endhint %}

### <mark style="color:orange;">Password Change Minimum Limit</mark>

The `PASS_MIN_DAYS` parameter in `/etc/login.defs` allows an administrator to prevent users from changing their password until a minimum number of days have passed since the last time the user changed their password. It is recommended that `PASS_MIN_DAYS` parameter be set to 7 or more days

Set the PASS\_MIN\_DAYS parameter to 7 in /etc/login.defs :

```
PASS_MIN_DAYS 7
```

Modify user parameters for all users with a password set to match

```
 chage --mindays 7 <user>
```

{% hint style="info" %}
You can also check this setting in /etc/shadow directly. The 4th field should be 7 or more for all users with a password.
{% endhint %}

### <mark style="color:orange;">Expiration Warning Days</mark>

The PASS\_WARN\_AGE parameter in `/etc/login.defs` allows an administrator to notify users that their password will expire in a defined number of days. It is recommended that the `PASS_WARN_AGE` parameter be set to 7 or more days.

Set the `PASS_WARN_AGE` parameter to 7 in `/etc/login.defs` :

```
PASS_WARN_AGE 7
```

Modify user parameters for all users with a password set to match:

```
chage --warndays 7 <user>
```

{% hint style="info" %}
You can also check this setting in /etc/shadow directly. The 6th field should be 7 or more for all users with a password.
{% endhint %}

### <mark style="color:orange;">Inactive Account Password Lock</mark>

User accounts that have been inactive for over a given period of time can be automatically disabled. It is recommended that accounts that are inactive for 30 days after password expiration be disabled.

find these accounts with:

```
 useradd -D | grep INACTIVE
```

Run the following command to set the default password inactivity period to 30 days:

```
 useradd -D -f 30
```

Modify user parameters for all users with a password set to match:

```
 chage --inactive 30 <user>
```

### <mark style="color:orange;">User Shell Timeout</mark>

Edit the /etc/bashrc and /etc/profile files (and the appropriate files for any other shell supported on your system) and add or edit any umask parameters as follows:

```
TMOUT=900
```

{% hint style="info" %}
The audit and remediation in this recommendation apply to bash and shell. If other shells are supported on the system, it is recommended that their configuration files also are checked. Other methods of setting a timeout exist for other shells not covered here.
{% endhint %}

### <mark style="color:orange;">Restrict Root Login to System Console</mark>

The file `/etc/securetty` contains a list of valid terminals that may be logged in directly as root.

Add the following line to the `/etc/pam.d/su` file:

```
auth required pam_wheel.so use_uid
```

Create a comma separated list of users in the wheel statement in the /etc/group file:

```
wheel:x:10:root,<user list>
```

### <mark style="color:orange;">Check for Empty Password Field</mark>

An account with an empty password field means that anybody may log in as that user without providing a password.

Run the following command and verify that no output is returned:

```
awk -F: '($2 == "" ) { print $1 " does not have a password "}' /etc/shadow
```

If any accounts in the /etc/shadow file do not have a password, run the following command to lock the account until it can be determined why it does not have a password:

```
passwd -l <username>
```

### <mark style="color:orange;">Check Root PATH Integrity</mark>

The root user can execute any command on the system and could be fooled into executing programs unintentionally if the PATH is not set correctly.

Including the current working directory (.) or other writable directory in root 's executable path makes it likely that an attacker can gain superuser access by forcing an administrator operating as root to execute a Trojan horse program.

Run the following script and verify no results are returned:

```
#!/bin/bash
if [ "$(echo "$PATH" | grep ::)" != "" ]; then
 echo "Empty Directory in PATH (::)"
fi
if [ "$(echo "$PATH" | grep :$)" != "" ]; then
 echo "Trailing : in PATH"
fi
p=$(echo "$PATH" | sed -e 's/::/:/' -e 's/:$//' -e 's/:/ /g')
set -- $p
while [ "$1" != "" ]; do
 if [ "$1" = "." ]; then
 shift
 continue
 fi
 if [ -d "$1" ]; then
 dirperm=$(ls -ldH "$1" | cut -f1 -d" ")
 if [ "$(echo "$dirperm" | cut -c6)" != "-" ]; then
 echo "Group Write permission set on directory $1"
 fi
 if [ "$(echo "$dirperm" | cut -c9)" != "-" ]; then
 echo "Other Write permission set on directory $1"
 fi
 dirown=$(ls -ldH "$1" | awk '{print $3}')
 if [ "$dirown" != "root" ] ; then
 echo "$1 is not owned by root"
 fi
 else
 echo "$1 is not a directory"
 fi
 shift
done
```

Correct or justify any items discovered in the Audit step.

## <mark style="color:red;">Give a User Sudo Privileges</mark>

The easiest way of doing this on a system set up with a general purpose administration group, like the Ubuntu system in this guide, is actually to just add the user in question to that group.

### <mark style="color:orange;">Debian-Based</mark>

```
sudo usermod -aG sudo username
```

The gpasswd command can also be used:

```
sudo gpasswd -a username sudo
```

### <mark style="color:orange;">Redhat-based</mark>

On CentOS, this is usually the wheel group instead of the sudo group:

```
sudo usermod -aG wheel username
```

Or, using gpasswd:

```
sudo gpasswd -a username wheel
```

## <mark style="color:red;">Editing sudoers File</mark>

#### The sudo command is configured through a file located at `/etc/sudoers`

{% hint style="danger" %}
Never edit this file with a normal text editor! Always use the visudo command instead!

Because improper syntax in the /etc/sudoers file can leave you with a system where it is impossible to obtain elevated privileges, it is important to use the visudo command to edit the file.
{% endhint %}

The visudo command opens a text editor like normal, but it validates the syntax of the file upon saving. This prevents configuration errors from blocking sudo operations, which may be your only way of obtaining root privileges.

```
visudo
```

If you would like to change the editor ssue the following command:

```
sudo update-alternatives --config editor
```

### <mark style="color:orange;">Default Lines</mark>

The first line, “Defaults env\_reset”, resets the terminal environment to remove any user variables. This is a safety measure used to clear potentially harmful environmental variables from the sudo session.

The second line, Defaults mail\_badpass, tells the system to mail notices of bad sudo password attempts to the configured mailto user. By default, this is the root account.

The third line, which begins with “Defaults secure\_path=…”, specifies the PATH (the places in the filesystem the operating system will look for applications) that will be used for sudo operations. This prevents using user paths which may be harmful.

### <mark style="color:orange;">User Privilege Lines</mark>

The fourth line, , which dictates the root user’s sudo privileges, is different from the preceding lines. Let’s take a look at what the different fields mean:

format:

```
user host=(run_as_user : run_as_group) command
```

<mark style="color:blue;">root</mark> ALL=(ALL:ALL) ALL

The first field indicates the username that the rule will apply to (root).

demo <mark style="color:blue;">ALL</mark>=(ALL:ALL) ALL

The first “ALL” indicates that this rule applies to all hosts.

demo ALL=(<mark style="color:blue;">ALL</mark>:ALL) ALL

This “ALL” indicates that the root user can run commands as all users.

demo ALL=(ALL<mark style="color:green;">:</mark><mark style="color:blue;">ALL</mark>) ALL

This “ALL” indicates that the root user can run commands as all groups.

demo ALL=(ALL:ALL) <mark style="color:blue;">ALL</mark>

The last “ALL” indicates these rules apply to all commands. This means that our root user can run any command using sudo, as long as they provide their password.

### <mark style="color:orange;">Set Up Custom Rules</mark>

The sudoers file can be organized more easily by grouping things with various kinds of “aliases”.

For instance, we can create three different groups of users, with overlapping membership:

```
User_Alias      GROUPONE = abby, brent, carl
User_Alias      GROUPTWO = brent, doris, eric
User_Alias      GROUPTHREE = doris, felicia, grant
```

Group names must start with a capital letter. We can then allow members of GROUPTWO to update the apt database by creating a rule like this:

```
GROUPTWO    ALL = /usr/bin/apt-get update
```

If we do not specify a user/group to run as, as above, sudo defaults to the root user.

We can allow members of GROUPTHREE to shutdown and reboot the machine by creating a “command alias” and using that in a rule for GROUPTHREE:

```
Cmnd_Alias      POWER = /sbin/shutdown, /sbin/halt, /sbin/reboot, /sbin/restart
GROUPTHREE  ALL = POWER
```

We create a command alias called POWER that contains commands to power off and reboot the machine. We then allow the members of GROUPTHREE to execute these commands.

### <mark style="color:orange;">Run As</mark>

We can also create “Run as” aliases, which can replace the portion of the rule that specifies the user to execute the command as:

```
Runas_Alias     WEB = www-data, apache
GROUPONE    ALL = (WEB) ALL
```

This will allow anyone who is a member of GROUPONE to execute commands as the www-data user or the apache user.

### <mark style="color:orange;">sudo without password</mark>

If we want to allow users to execute it with root privileges without having to type a password, we can make a rule like this:

```bash
/user ALL=(ALL) NOPASSWD: ALL
GROUPONE    ALL = NOPASSWD: /usr/bin/updatedb
```

### <mark style="color:orange;">Tags</mark>

A tag is relevant for the rest of the rule unless overruled by its “twin” tag later down the line.

```bash
GROUPTWO    ALL = NOPASSWD: /usr/bin/updatedb, PASSWD: /bin/kill
```

Another helpful tag is <mark style="color:orange;">NOEXEC</mark>, which can be used to prevent some dangerous behavior in certain programs.

For example, some programs, like “less”, can spawn other commands by typing this from within their interface:

```bash
!command_to_run
```

This basically executes any command the user gives it with the same permissions that “less” is running under, which can be quite dangerous.

To restrict this, we could use a line like this:

```bash
username  ALL = NOEXEC: /usr/bin/less
```

### <mark style="color:orange;">disable sudo timer</mark>

In order to set a different timeout (globally) than the default (=15 min) you can edit `/etc/sudoers`:

```bash
sudo visudo                                       # opens /etc/sudoers for editing
# change the following line:
# Defaults    env_reset
# to:
Defaults        env_reset,timestamp_timeout=30    # timeout in minutes
```

or:

```bash
cd /etc/sudoers.d
sudo visudo -f username
Defaults        env_reset,timestamp_timeout=30    # timeout in minutes
```

Special values:

* `-1`: no timeout
* `0`: get prompted every single time

## <mark style="color:red;">Prevent Shell Escapes</mark>

Certain programs, especially text editors and pagers, have a handy shell escape feature. This allows a user to run a shell command without having to exit the program first.

### <mark style="color:orange;">Set sudoedit as a Text Editor</mark>

#### some of the programs that have shell escape feature:

* nano
* vi
* vim
* less
* emacs
* view
* more
* ...

to prevent user from shell escape with text editors, give them sudo permissions for `sudoedit` only, sudoedit has no shell escape feature

```
frank ALL=(ALL) sudoedit /etc/ssh/sshd_config
```

### <mark style="color:orange;">Limiting the user's actions with commands</mark>

Let's say that you create a sudo rule so that Sylvester can use the systemctl command:

```bash
sylvester ALL=(ALL) /usr/bin/systemctl
```

This allows Sylvester to have full use of the systemctl features. He can control daemons, edit service files, shut down or reboot, and carry out every other function that systemctl does. That's probably not what you want. It would be better to specify what systemctl functions that Sylvester is allowed to do. Let's say that you want him to be able to control just the Secure Shell service.

he syntax would be like this:

```bash
useraccount run-what=(run-as) command * parameters
```

You can make the line look like this:

```bash
sylvester ALL=(ALL) /usr/bin/systemctl * sshd
```

Sylvester can now do everything he needs to do with the Secure Shell service, but he can't shut down or reboot the system, edit other service files, or change systemd targets. But what if you want Sylvester to do only certain specific actions with the Secure Shell service? Then you'll have to omit the wildcard and specify all of the actions that you want Sylvester to do:

```
sylvester ALL=(ALL) /usr/bin/systemctl status sshd, /usr/bin/systemctl
restart sshd
```

Now, Sylvester can only restart the Secure Shell service or check its status.
