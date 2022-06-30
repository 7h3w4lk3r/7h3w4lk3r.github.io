# 🔴 C2 Frameworks

## <mark style="color:red;">Red Teaming</mark>

&#x20;_**Red Teaming is a full-scope, multi-layered attack simulation designed to measure how well a company’s people and networks, applications and physical security controls can withstand an attack from a real-life adversary.**_

During a red team engagement, highly trained security consultants enact attack scenarios to reveal potential physical, hardware, software and human vulnerabilities. Red team engagements also identify opportunities for bad actors and malicious insiders to compromise company systems and networks or enable data breaches.



It could be broadly divided into these categories.

* <mark style="color:green;">**Reconnaissance**</mark> – Gathering information about employees, Managers, Technology stack, Security appliances, etc using passive and active recon techniques
* <mark style="color:green;">**Physical security**</mark> -Physically mapping the building, Bypassing or cloning RFID/Biometrics, Lockpicking, etc
* <mark style="color:green;">**Social Engineering**</mark> – Malware delivery/Credentials logging via targeted phishing, Gaining entry to the building/ODC, Fake surveys, Planting network implants, Dumspter diving, etc
* <mark style="color:green;">**Exploitation**</mark> – Payload delivery & execution, Stealthy C\&C server communication
* <mark style="color:green;">**Persistence**</mark> – Privilege Escalation, Maintaining persistence & Pivoting
* <mark style="color:green;">**Post-Exploitation**</mark> – Lateral movement & Data exfiltration

{% hint style="info" %}
In a red team engagement a C2 is the most used tool for collaboration and  simulating a real-world scenario.
{% endhint %}

## <mark style="color:red;">Command & Control (C2)</mark>

Although Red Teams use similar offensive security tools to that of penetration testers, there are tools more emphasized by Red Teams, specifically when it comes to command and control. While other security testers may use command and control tools as well, a Red Team’s goals are typically heavily dependent on a solid C2 infrastructure and toolset.

**Command and Control** (**C2**) is a dedicated tactic in the Mitre ATT\&CK framework consisting of different techniques, each describing different ways to achieve a persistent connection between the 'implants' (software agents running on exploited workstations providing the ability to run commands and retrieve results) and the command and control server.

Choosing the C2 protocol is not a trivial task as, in most cases, this governs whether a persistent connection will be established back to the attacker. Select a protocol operating on a non-standard port (trying to get an HTTPS connection out on ports  8080 or 8443), or make uneducated assumptions, (assuming there is no proxy present on the exploited organization's  infrastructure) and, no matter the sophistication of the attack delivery or payload, you will still end up empty-handed.

Having said all that, one can - in the vast majority of cases - count on two protocols being allowed to communicate with external servers, namely HTTP(S) and DNS, so a very large part of implants use one of these protocols to reach their C2 servers.

## <mark style="color:red;">C2 Matrix</mark>

#### Find out which C2 fits your needs.

{% embed url="https://www.thec2matrix.com/" %}

## <mark style="color:red;">**Common C2 Frameworks**</mark>

### <mark style="color:orange;">**Cobaltstrike**</mark>

Cobaltstrike is one of the most used platforms worldwide that allows the deployment of a beacon agent on the victim’s machine. This kind of agent provides a lot of functionalities, including keylogging, file upload and download, socks proxy, VPN deployment, privilege escalation techniques, mimikatz, port scanning and the most advanced lateral movements.

The Cobalstrike beacon uses a file-less approach under a stageless or multistage shellcode typically loaded by exploring a vulnerability or a weakness that will execute in memory the final payload without touching the disk. It supports C2 connection via HTTP, HTTPS, DNS and SMB protocols. In addition, Cobalstrike also provides a development toolkit called Artifact Kit that facilitates customized shellcode loaders.

### <mark style="color:orange;">Covenant</mark>

Covenant is a collaborative C2 framework designed essentially for red teaming assessments. This post-exploitation framework supports .NET core and is cross-platform. It supports Windows, macOS and Linux-based OS. Covenant also provides a pre-configured Docker image to facilitate its installation.&#x20;

The Covenant agent known as “Grunt” is developed in C#, and it can be customized directly on the C2 web portal. Many features are provided, such as lateral movement techniques, mimikatz, exfiltration scripts and so on. More details about this open-source project can be found on the [GitHub page](https://github.com/cobbr/Covenant/wiki/).

### <mark style="color:orange;">Sillenttrinity</mark>

Sillenttrinity is an asynchronous and multi-server command and control framework developed in Python3 and .NET DLR. This platform uses embedded third-party .NET scripting languages and dynamically calls .NET APIs; the author entitled BYOI (Bring Your Own Interpreter) technique.

This tool uses a dedicated team server with many modern features into a powerful C2 framework. The usage of WebSockets allows effective real-time communication between the team server and the agents installed on the victims’ machines. In detail, SILENTTRINITY uses Ephemeral Elliptic Curve Diffie-Hellman Key Exchange to encrypt all C2 traffic between the team server and its implant.

One of the appreciated features is its modularity — users can modify any module easily.

**URL**: [https://github.com/byt3bl33d3r/SILENTTRINITY](https://github.com/byt3bl33d3r/SILENTTRINITY)

### <mark style="color:orange;">Koadic</mark>

Koadic is a Windows post-exploitation framework that uses Windows Script Host (JScript/VBScript) and supports all Windows versions from Windows 2000 to Windows 11.

Koadic is an open-source tool and is available on [Github](https://github.com/zerosum0x0/koadic). This framework is written in Python, and its payloads are JavaScript-based with XOR encryption. The agents are installed on the target machines using the default mshta or using the default mshta or Microsoft HTML Application stagers.&#x20;

**URL**: [https://github.com/zerosum0x0/koadic](https://github.com/zerosum0x0/koadic)



### <mark style="color:orange;">Metasploit</mark>

The Metasploit framework is a popular tool distributed along with Kali Linux distribution and can be used to find vulnerabilities on networks and servers. As it is an open-source tool, it can be customized by operators and used with many operating systems, including Android, iOS, macOS, Linux, Windows, Solaris, etc.

Meterpreter is equipped with many features, including staged and non-staged payloads to enable port forwarding between networks.

### <mark style="color:orange;">Merlin</mark>

Merlin is a C2 that uses HTTP/1.1, HTTP/2 and HTTP/3 protocols to evade detection and communicate with its agents. Merlin is a cross-platform tool written in Goland and capable of working in several operating systems.

Each merlin compilation will generate unique payloads capable of avoiding AV detection from the detection point-of-view, as[ demonstrated in this article](https://resources.infosecinstitute.com/topic/using-merlin-agents-to-evade-detection/). It uses a client-server architecture and provides the most advanced features of red teaming presented on other C2 frameworks in the market. As it is an open-source project, operators can customize Merling agents, their _modus operandi_ and how it is loaded into the memory.

**URL**: [https://github.com/Ne0nd0g/merlin](https://github.com/Ne0nd0g/merlin)

## <mark style="color:red;">Forwarders / Redirectors</mark>

![](<../../.gitbook/assets/image (185).png>)

When creating a command-and-control infrastructure, it is common for the callbacks to not communicate directly to the attacker’s C2 server. Many times, they will go through a compromised webpage, or a fake site used as a redirector.&#x20;

**A redirector is basically a server that will take requests and forward them to another address, such as the real malicious server. This is to hide the underlying attacker address if the C2 traffic is ever discovered.**

Have you ever analyzed a web address that was flagged as malicious, only to see a seemingly benign or a 404-error page? This may indicate that the page is not malicious or no longer existing, but it could also indicate the page is being used as a redirector/proxy.

Apache, Nginx, and other web servers have the ability to proxy/redirect traffic when desired conditions are met. This is useful from an attacking perspective as it can add an extra layer of obfuscation to an analyst observing the traffic.
