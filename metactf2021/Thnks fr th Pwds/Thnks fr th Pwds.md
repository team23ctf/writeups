# MetaCTF 2021
## Thnks fr the Pwds (100pts)
### Description: 
>On a red team engagement, you discover a text file on an administrator’s desktop with all of their passwords - you now have the keys to the kingdom!
During the engagement debrief, you explain what you found and how you were able to access so many systems. The administrator says that's impossible, because they encrypted all of the passwords in the file.
Here’s an example of one of their “encrypted” passwords: `TWV0YUNURntlbmNvZGluZ19pc19OMFRfdGhlX3NhbWVfYXNfZW5jcnlwdGlvbiEhfQ==`
See if you’re able to recover the Administrator's password.

### Step 1 - Identify Encoding:
Based on the '==' padding at the end, we may be able to identify this as Base64 encoding.

### Step 2 - Decode:
Sure enough, it decoded properly.
<br /> <br />
![image](https://user-images.githubusercontent.com/43623870/144763086-4c79c0be-5461-4f1c-8881-92386a03f3c7.png)

<details>
  <summary>Flag Spoiler</summary>
  MetaCTF{encoding_is_N0T_the_same_as_encryption!!}
</details>

## Learning Takeaways
We learn how to identify and decode Base64 strings.
