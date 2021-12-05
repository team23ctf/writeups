# Cyber Security Rumble 2021
## Challenge - Result (Level 1)
### Description: 
> I really want to know my test result, but unfortunately its additionally protected. I attached the email. Maybe you can help?
### Included Files:
> [result.tar.gz](https://github.com/zylideum/writeups/blob/main/csr2021/result/Test_Result.eml)
### Step 1
First, I analyzed the contents of the conversation rather than any headers or excess information.
What stood out was the German and English translations of the doctor's communications:

```html
<h3>Deutsch</h3>
<h1>Hallo Hans Deutschmeister</h1>
<p>Ihr Testergebnis liegt nun vor und befindet sich im Anhang dieser Mail.</p>
<p>Das Ergebnis-PDF ist verschlesselt und ist durch ein Passwort zustzlich geschetzt.<br />
<strong>Das Passwort ist Ihre persenliche Postleitzahl</strong></p>
<p>Wir enschen Ihnen alles Gute und viel Gesundheit!<br /=>
Ihr CoviMedical-Team</p>
              
<h3>English</h3>
<h1>Hi Hans Deutschmeister</h1>
<p>Your test result is now available and can be found in the attachment to this email.</p>
<p>The result PDF is encrypted and is additionally protected by a password.<br />
<strong>The password is your personal zipcode</strong>.</p>
<p>We wish you all the best and good health!<br />
Your CoviMedical Team</p>
```
Great, so now we know we have an attached PDF, we know it's encrypted, and we know the password is our personal zipcode.

### Step 2
Our next step should be retrieving this PDF from the .EML file and reconstructing it. Fortunately, it's not too difficult to find as it's clearly labelled under the `Content-Type` header.
Since the attachment is long, I'll only include the final portion:
```
...MTMgL0lEIFs8YTdlN2YzMDQ0YWUwMjNmMWQxZmFjNDQ4YmYyNTU5MmM+PDJj
MTYwZmMzNjkxZTg1ZmFmMjdkMjBlNmFhM2RlYmM3Pl0gL0VuY3J5cHQgMTIgMCBSID4+CnN0YXJ0
eHJlZgoxMDE5OQolJUVPRgo=
```
This is clearly just base64 encoded data, so we can go ahead and decode that to see if it has any pertinent information:

![image](https://user-images.githubusercontent.com/43623870/143732036-eb3cde61-ac30-40df-a167-0204eb8fe57c.png)

Nothing stands out as unusual, but it is clear that this should be a complete file, as we can see the `PDF` identifier and `%%EOF` at the end, denoting 'End of File'
From here, we can reconstruct the PDF file by copying this decoded base64 data into a new `.pdf` file.
Sure enough, we can open it and it prompts us for a password like the email suggested.

![image](https://user-images.githubusercontent.com/43623870/143732113-a7259cd2-0f45-4133-bbf9-5ec510992c9c.png)

### Step 3
Now we just need the password to the PDF file, which can be cracked with john2pdf. Since we know that the password is our 'personal zipcode', we can also create a custom wordlist of all zipcodes in the world.

Using john2pdf yielded the following hash which I saved to use john with our wordlist:
```
$pdf$5*6*256*-4*1*16*a7e7f3044ae023f1d1fac448bf25592c*48*67171ef681ef91ed5bea716fa5eceda89b8659e1e7e4c5b810be37befb1ddd6ccc0217015a1eebce2c84e57607d22225*48*9b5a9e500fb87e9d4171e22fa77eb5a72ba857d2e2eaa4674ec8ed69831c77fdf400be271c304f6875dd1c5272a3fef0*32*e6e5cbe1bef4ba12e74a43e8a079970ed93664333955e76d22396fe69367d3fc*32*3de89fe5d937cf8a0b1105dd7c60a0c24bd3ca0e815b59cd02a59581d36165be
```
Using Google, I was able to find a [list](https://github.com/Zeeshanahmad4/Zip-code-of-all-countries-cities-in-the-world-CSV-TXT-SQL-DATABASE) of all of the countries, cities, and zipcodes in the world. It's quite a large file and has extraneous information, so I used the following command to carve out only zipcodes:
```bash
cat allCountriesTXT.txt | awk '{print $2}' > zipcodes.txt
```
I also verified this data was formatted properly by using the following command (with my zipcode expunged):
```bash
cat allCountriestTXT.txt | awk '{print $2,$3}' | grep [your zipcode]
```
Sure enough, it listed my zipcode followed by my city, so I was confident that all zipcodes were now carved and ready to be used as a wordlist.

### Step 4
Finally, cracking the PDF is now relatively simple, as we just need to run john with the following command:
```bash
john --wordlist=zipcodes.txt pdf.hash
```
and once it was done cracking:
```bash
john --show pdf.hash
```
John was successful, and we have our password which can now be used to access the file:
<details>
  <summary>Cracked PDF</summary>
  
  `result.pdf:73760`
  
  ![image](https://user-images.githubusercontent.com/43623870/143732546-c6bab04a-f424-4f72-80b6-2a99e2f9bcdf.png)
</details>

## Summary
In this challenge, we deciphered and rebuilt the attachment of an .eml file and used a custom wordlist to crack it.
Had the password been more secure and perhaps set by the user, this would not have been possible. Thank "covimedical" for essentially spilling the password via plaintext.


