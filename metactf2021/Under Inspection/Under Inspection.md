# MetaCTF 2021
## Under Inspection (100pts)
### Description: 
>Someone made this [site](https://metaproblems.com/2841e99cee26f773b26b300acad556c4/inspect/) for the Autobots to chat with each other. Seems like the Decepticons have found the site too and made accounts.
One of the Autobot accounts has a flag that they're trying to keep hidden from the Decepticons, can you figure out which account it is and steal it?

### Step 1 - View Source Code:
By viewing the source code, we can see the `loginSubmission()` function which stores usernames and passwords in plaintext:
```javascript
var username = document.getElementById("username").value;
var password = document.getElementById("password").value;
var result = document.getElementById("result");
var accounts = [
  {user: "Admin", pwd: "MetaCTF{super_secure_password}"},
  {user: "Bumblebee", pwd: "MetaCTF{sting_like_a_bee}"},
  {user: "Starscream", pwd: "MetaCTF{the_best_leader_of_the_decepticons}"},
  {user: "Jazz", pwd: "MetaCTF{do_it_with_style_or_dont_do_it_at_all}"},
  {user: "Megatron", pwd: "MetaCTF{peace_through_tyranny}"},
];
```
Since these are all `MetaCTF{}` formatted flags, we cannot be certain which one is the correct one. Further down in this function, we can see a validation that only validates Jazz:
```javascript
for(var a in accounts) {
  if(accounts[a].user == username && accounts[a].pwd == password) {
    if(username == "Jazz") {
      result.innerHTML = "Welcome, Jazz. The flag is " + password;
    } else {
      result.innerHTML = "Welcome, " + username + ".";
    }
return false;
```
It appears that logging in to Jazz validates that the flag is indeed is password. This suites the challenge theme of finding an Autobot flag rather than a Decepticon's.

<details>
  <summary> Flag Spoiler </summary>
  MetaCTF{do_it_with_style_or_dont_do_it_at_all}
</details>

## Learning Takeaways
We learned that plaintext usernames and passwords should not be displayed in the source code.
