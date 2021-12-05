# MetaCTF 2021
## Wrong Way on a One Way Street (100pts)
### Description: 
>Hashing is a system by which information is encrypted such that it can never be decrypted... theoretically. Websites will often hash passwords so that if their passwords are ever leaked, bad actors won't actually learn the user's password; they'll just get an encrypted form of it. However, the same password will always hash to the same ciphertext, so if the attacker can guess your password, they can figure out the hash. Can you guess the password for this hash? `cb78e77e659c1648416cf5ac43fca4b65eeaefe1`
### Step 1 - Understand the Ask:
Given a hash, can we crack it? There's a long, more robust way to do this and a short way. In this writeup, I'll be covering the short way, but if you're interested in more advanced password cracking, look into tools like [hashcat](https://hashcat.net/hashcat/) or [John](https://www.openwall.com/john/).
### Step 2 - Speedy Cracking:
If we don't want to spend time gathering a wordlist, opening our VM, running tools, and making our GPU cry, there's a website called [crackstation](https://crackstation.net/) that has pre-computed hashes for cracking. It's not the best tool if you have complex passwords or custom needs, but it will crack simple and short ones fast given it's in their database.

![image](https://user-images.githubusercontent.com/43623870/144769056-9f0e899b-1129-405f-af24-fab1cf99c548.png)

When I identify a hash, I usually throw it in crackstation first to see if it was a simple password before dedicating my resources to it.



<details>
  <summary> Flag Spoiler </summary>
  babyloka13
</details>

## Learning Takeaways
We learned a bit about hashing and password cracking in this challenge, and how to get away with the least amount of effort for a cracking attempt.
