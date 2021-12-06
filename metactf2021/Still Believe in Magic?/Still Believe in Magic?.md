# MetaCTF 2021
## Still Believe in Magic? (150pts)
### Description: 
>We found [an archive with a file in it](https://metaproblems.com/f03e38955de03e3d860d32dfd20b132f/magic.tar.gz), but there was no file extension so we're not sure what it is. Can you figure out what kind of file it is and then open it?
### Included Files:
>[magic.tar.gz](https://github.com/team23ctf/writeups/blob/main/metactf2021/Still%20Believe%20in%20Magic%3F/magic.tar.gz)

### Step 1 - File Reconnaissance:
First, use `gunzip` and `tar -xvf` to unzip and unpack the file like you normally would.
<br /> <br />
Any time we're given a file, I recommend using `file`, `cat`, and `strings` before doing anything else.

![image](https://user-images.githubusercontent.com/43623870/144797384-2f3f6799-a3b9-4f09-b446-41adfa96f18f.png)

The output here already has interesting information, `file` claims it's a ZIP archive, and `cat` shows it may be related to MacOSX. Strings cleans up our cat a bit:

![image](https://user-images.githubusercontent.com/43623870/144797603-7542c3d1-98b6-4cfc-92a7-f938d3c8cb34.png)

To verify it's actually a ZIP archive, we ran `xxd -d` which dumps the hex and raw bytes of the file contents. 

![image](https://user-images.githubusercontent.com/43623870/144797763-8b10f231-2c1f-4f38-93e0-2fe81e4d7f19.png)

The first few bytes are called `magic bytes` because they're the file identifier in most cases. In fact, that's part of what the `file` command does - identifies the magic bytes and checks them.
In our case, we can see our file begains with `504b` or `PK` in ASCII, and we can check that against a [list of file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures).
When looking at `50 4b` or `PK`, we can manually verify it is the signifier of a ZIP archive.

### Step 2 - Unzip the ZIP:
Now that we know it's a ZIP archive, the logical step would be to unzip it. Sure enough, we unzip it and extract `magic.txt` which holds the contents of the flag.
Likely, the MacOSX information was there because this ZIP file was created on OSX.


<details>
  <summary> Flag Spoiler </summary>
  MetaCTF{was_it_a_magic_trick_or_magic_bytes?} 
</details>

## Learning Takeaways
Trust `file` to an extent - sometimes a specially crafted file might not be what it appears to be. We can use our own knowledge to determine a file type and figure out what to do with it from there.
