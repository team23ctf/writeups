# MetaCTF 2021

## Easy as it (TCP) Streams (250pts)

### Description:

> Caleb was designing a problem for MetaCTF where the flag would be in the telnet plaintext. Unfortunately, he accidentally stopped the [packet capture](https://metaproblems.com/46dc63e7dbfa1ca757a459063dff0959/easy_as_it_streams.pcapng) right before the flag was supposed to be revealed. Can you still find the flag? Note: You'll need to decrypt in CyberChef rather than using a command line utility.

### Included Files:

> [easy_as_it_streams.pcapng](https://github.com/team23ctf/writeups/blob/main/metactf2021/Easy%20as%20it%20(TCP)%20Streams/easy_as_it_streams.pcapng)

### Step 1 - Locating the Encrypted Message

Upon opening the provided PCAP file, using context clues from the title, we right clicked on the first TCP packet and went `Follow > TCP Stream` as shown below.

![Part 1](https://user-images.githubusercontent.com/30860555/144777340-da5bc4ff-1681-4cd3-a8f8-2daee3365561.png)

After doing so, we find the encrypted PGP message below:

![Part 2](https://user-images.githubusercontent.com/30860555/144775848-c6cd97da-f698-4367-a641-21791434dc92.png)

Now that we have the encrypted PGP message, we need to find a key to decrypt this.

### Step 2 - Locating Key Information (Pun Intended)

Heading back into Wireshark, I incremented the TCP stream to `1` to see what other information I could find.

![Part 3](https://user-images.githubusercontent.com/30860555/144775852-b1928a5d-1bd6-48de-899d-fab80b7d385a.png)

After right-clicking on the first packet and following this new TCP stream, I found the private PGP key!

![Part 4](https://user-images.githubusercontent.com/30860555/144775860-a40fb8db-007f-4244-84a8-df29c9730ac1.png)

Unfortunately, this private PGP key can't be used on its own. It is password protected so we still needed to locate some sort of password.

### Step 3 - Locating the Private PGP Key's Password

Heading back into Wireshark, I incremented the TCP stream one more time to `2`, right-clicked on the first packet, and followed a new TCP stream again. This time, I found a telnet session, though it is hard to read as is.

![Part 5](https://user-images.githubusercontent.com/30860555/144777263-392039d0-6c4c-40a6-bb7d-f3ec4b75be0c.png)

In order to make above more readable, at the bottom of the window, I switched from the `Entire Conversation` to `10.0.2.6:23 -> 10.0.2.9:59704 (1213 bytes)` to read the blue text alone.

![Part 6](https://user-images.githubusercontent.com/30860555/144775870-ef855e5f-ae66-4d61-ae69-4424fb61f3a5.png)
![Part 6 Amended](https://user-images.githubusercontent.com/30860555/144777142-79e81fa7-907e-4372-9044-5c1d37949c8c.png)

When examined closer, we can see the command that was used to create the private PGP key.

![Part 7](https://user-images.githubusercontent.com/30860555/144777194-fdfeaaa9-2307-4910-945f-3b784fa6935f.png)

In the command, we see the passphrase `farnha`. This is all the information we need to crack the PGP message!

## Step 4 - Cracking the PGP Message

In order to crack the PGP message, we booted up [CyberChef](https://gchq.github.io/CyberChef/). We dragged the `PGP Decrypt` module into the `Recipe` section and filled in the private PGP key and private password. We put the PGP message in the `Input` section in the top right.

![Part 8](https://user-images.githubusercontent.com/30860555/144775883-1134469e-c6b3-4d44-b88c-227a583e23a8.png)
![Part 9](https://user-images.githubusercontent.com/30860555/144775885-ec4d9243-c29e-4616-b2e0-4e9458a5cce7.png)

Unfortunately, the flag isn't quite usable as of yet. To fix this, we dragged in the `Magic` module to the bottom of the recipe. 

**THE FLAG IS SHOWN IN THE BELOW IMAGE, PROCEED IF YOU WISH**

![Part 10](https://user-images.githubusercontent.com/30860555/144775892-e3d9d57f-f620-4a7a-b69b-d2c9431317d0.png)

<details>
  <summary> Flag Spoilers</summary>
  MetaCTF{cleartext_private_pgp_keys}
</details>

## Learning Takeaways

We learned that following TCP streams is a great way to find information. To search through all TCP streams in a PCAP file, it is helpful to increment the stream at the top of the Wireshark window so we don't have to go searching through the PCAP for all of the TCP streams. We also learned that CyberChef is incredibly powerful for decryption.
