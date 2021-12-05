# MetaCTF 2021
## This Ain't a Scene, It's an Encryption Race (100pts)
### Description: 
>Ransomware attacks continue to negatively impact businesses around the world. What is the Mitre ATT&CK technique ID for the encryption of data in an environment to disrupt business operations?
The flag format will be `T####`.

### Step 1 - Resource Gathering:
We can use the [MITRE ATT&CK](https://attack.mitre.org/techniques/enterprise/) framework to identify the technique for encryption of data.
By navigating to "Impact", since this encryption technique disrupts business operations, we can find the technique ID relatively quickly. 

![image](https://user-images.githubusercontent.com/43623870/144764551-47a24eae-8ae3-4aff-a3a2-064329b67e36.png)

If you're uncertain, the description makes it clear that it causes business disruption.

<details>
  <summary> Flag Spoiler </summary>
  T1486
</details>

## Learning Takeaways
We learned about the MITRE ATT&CK framework and how to navigate it, as well as how to find techniques.
