# MetaCTF 2021
## Sharing Files and Passwords (150pts)
### Description: 
>FTP servers are made to share files, but if its communications are not encrypted, it might be sharing passwords as well. The password in [this pcap](https://metaproblems.com/2dd6443361555f266a8c2f54c50d01e9/ftp_challenge.pcapng) to get the flag
### Included Files:
>[ftp_challenge.pcapng](https://github.com/zylideum/writeups/blob/main/metactf2021/Sharing%20Files%20and%20Passwords/ftp_challenge.pcapng)


### Step 1 - Explore Pcap Data:
As with every `.pcap` or `.pcapng` file, we want to open it in [Wireshark](https://www.wireshark.org/) and explore the packets first.
<br /> <br />
![image](https://user-images.githubusercontent.com/43623870/144773977-6bcfe767-bfe6-4865-bce5-f9bdc739f5b6.png)

We can see there is indeed FTP communications in the packet capture, so we can filter our search and begin to look for a plaintext flag.

### Step 2 - Filter and Find:
By using `ftp` in the filter bar, we can filter to look at only FTP connections. We can now right-click and follow the TCP stream (since FTP uses TCP for connection) and see the requests and responses being made between the two clients.

![image](https://user-images.githubusercontent.com/43623870/144774095-7f429f1a-397e-4a31-8957-ff3e56d3ab7f.png)

It does indeed look like there is a password being sent in plaintext over the network, and we can now use that to solve.

Additionally, we could have just looked at the info column without following any streams, as the password is also shown there.

![image](https://user-images.githubusercontent.com/43623870/144774197-f5d9e351-15f0-4038-9973-47e1443e610a.png)

<details>
  <summary> Flag Spoiler </summary>
  ftp_is_better_than_dropbox
</details>

## Learning Takeaways
We learned that FTP may send unencrypted credentials over a network - it's important to use SFTP or an equivalent secure method of transferring files to prevent malicious users from having access to credentials.
