# Trabalho realizado na Semana #3

## Identificação - CVE-2020-0796

- CVE-2020-0796 is a bug in the compression mechanism of SMBv3.1.1 (SMB is a network file sharing protocol), also known as SMBGhost or CoronaBlue.

- This CVE is a critical remote code execution vulnerability in the Microsoft Server Message Block 3.1.1 protocol.

- An SMB Header is used when sending compressed messages, containing paremeters that can be exploited by an integer overflow.

- This vulnerability would affect the versions: Windows 10 v1903, Windows 10 v1909, Windows Server v1903, Windows Server v1909.

## Catalogação

- The vulnerability was first reported on the 10th of March 2020 by the security system of Microsoft. Two days after, a new patch was released that would solve this.

- An attacker could exploit this vulnerability to execute arbitrary code on the side of the SMB server or client.

- This CVE was given a base score of 10.0 in the Severity and Metrics scale, by National Vulnerability Database (NVD) analysts.

- Since Microsoft noticed and solved this vulnerability, there were no bug-bounties regarding it.

## Exploit

- To exploit the vulnerability against a client, an unauthenticated attacker would need to configure a malicious SMBv3 server and convince an user to connect to it. Against a server, an unauthenticated attacker could send a specially crafted SMBv3 packet to the server.

- The exploit consists of creating a compressed data packet with a malformed header that causes an integer overflow. This exposes the weakness of improper restriction of operations within the bound of a Memory Buffer (CWE-119).

- More specifically, there are two parameters (OriginalCompressedSegmentSize and offset) in the header that determine the size of an allocated buffer.

- Since the calculated buffer size is assumed to be an unsigned long, a negative value gets cast into a large unsigned number, hence the decompression routine decompresses a buffer bigger than the original size.

## Ataques

- Recent Microsoft Windows versions randomize memory allocation of processes by default. This randomization significantly increases the difficulty of successful exploitation of memory corruption vulnerabilities such as CVE-2020-0796. 

- Consequently, there are no reports of succesful attacks but there are many proof of concepts using LPE, RCE and [DoS](https://www.youtube.com/watch?v=_DMDeAG9-KU). 

- If this vulnerability wasn't quickly solved, a RCE attack could be used to get control of a system and, for example, steal data, infiltrate and monitor the system or use CPU resources, as shown [here](https://github.com/ZecOps/CVE-2020-0796-RCE-POC).
