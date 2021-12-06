# MetaCTF 2021
## There Are No Strings on Me (100pts)
### Description: 
>We've got this program that's supposed to check a password, and we're not quite sure how it works. Could you take a look at it and see about finding the password it's looking for?
### Provided Files:
>[download](https://github.com/team23ctf/writeups/blob/main/metactf2021/There%20Are%20No%20Strings%20on%20Me/strings)

### Step 1 - Identify File:
When downloading CTF files, there are a couple of things that should be done. First, `file` should be ran to determine what kind of file it is.

![image](https://user-images.githubusercontent.com/43623870/144769708-8bcde4d9-b4c2-4cb3-a134-9257cd9c1c9d.png)

Now that we know it's an executable, we could `chmod +x` and start running it, but there is more information we should gather before doing so.

### Step 2 - Run Strings:
Strings is another critical command-line tool that can be used to print human-readable information. Although this is a compiled binary, we may still be able to get information out of the file.

![image](https://user-images.githubusercontent.com/43623870/144769800-c672ab8d-f44e-4608-bb13-82805c4e8036.png)

Sure enough, we can see a flag, as well as various function names and calls. Now we can `chmod +x` and verify that this actually works with the program.

![image](https://user-images.githubusercontent.com/43623870/144769860-db12de3b-1022-4bdc-ac08-b345558a5eee.png)

This verifies that the 'password' is correct and we have the right flag.

<details>
  <summary> Flag Spoiler </summary>
  MetaCTF{this_is_the_most_secure_ever}</details>
</details>

## Learning Takeaways
We learned about the string command and how various information can be found in compiled binaries using the command. This is why many challenges host a flag on a remote host, to make it more challenging to receive. Anything compiled with the name of the flag will be displayed with strings!
