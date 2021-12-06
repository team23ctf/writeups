# MetaCTF 2021

## Easy as it (TCP) Streams (50pts)

### Description:

> Caleb was designing a problem for MetaCTF where the flag would be in the telnet plaintext. Unfortunately, he accidentally stopped the [packet capture](https://metaproblems.com/46dc63e7dbfa1ca757a459063dff0959/easy_as_it_streams.pcapng) right before the flag was supposed to be revealed. Can you still find the flag? Note: You'll need to decrypt in CyberChef rather than using a command line utility.

### Included Files:

> \[easy_as_it_streams.pcapng\]\(url - upload in github chall folder\)

### Step 1 - Locating the Encrypted Message

Upon opening the provided PCAP file, using context clues from the title, right clicked on the first TCP packet and went `Follow > TCP Stream` as shown below.

Part 1.png

After doing so, we find the encrypted PGP message below:

Part 2.png

Now that we have the encrypted PGP message, we need to find a key to decrypt this.

### Step 2 - Locating Key Information (Pun Intended):

Heading back into Wireshark, I incremented the TCP stream to `1` to see what other information I could find.

Part 3.png

After right-clicking on the first packet and following this new TCP stream, I found the private PGP key!

Part 4.png

Unfortunately, this private PGP key can't be used on its own. It is password protected so we still needed to locate some sort of password.

### Step 3 - Locating the Private PGP Key's Password

Heading back into Wireshark, I incremented the TCP stream one more time to `2`, right-clicked on the first packet, and followed a new TCP stream again. This time, I found a telnet session, though it is hard to read as is.

Part 5.png

In order to make above more readable, at the bottom of the window, I switched from the `Entire Conversation` to `10.0.2.6:23 -> 10.0.2.9:59704 (1213 bytes)` to read the blue text alone.

Part 6.png

Above, when examined closer, we can see the command that was used to create the private PGP key.

Part 7.png

In the command, we see the passphrase `farnha`. This is all the information we need to crack the PGP message!

## Step 4 - Cracking the PGP Message

In order to crack the PGP message, we booted up [CyberChef](https://gchq.github.io/CyberChef/). We dragged the `PGP Decrypt` module into the `Recipe` section and filled in the private PGP key and private password. We put the PGP message in the `Input` section in the top right.

Part 8.png
Part 9.png

As you can see above, the flag isn't quite usable this way. To fix this, we dragged in the `Magic` module to the bottom of the recipe.

<details>
  <summary> Fixed Output/Flag Spoiler </summary>
  Part 10.png
</details>

<details>
  <summary> Flag Spoilers</summary>
  MetaCTF{cleartext_private_pgp_keys}
</details>

## Learning Takeaways

We learned that following TCP streams is a great way to find information. To search through all TCP streams in a PCAP file, it is helpful to increment the stream at the top of the Wireshark window so we don't have to go searching through the PCAP for all of the TCP streams. We also learned that CyberChef is incredibly powerful for decryption.
