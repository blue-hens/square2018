## Square CTF: GDPR -> C5: de-anonymization

### Challenge: 
C5 is an [online system](https://glacial-coast-79626.squarectf.com/4WzKpfyFbgdEzO3ONxDPpIXdo9Qps5) and is thus very simple to disable. You just need to login as the Captain Yakubovics. Too bad she’s no longer around to [hand you her password](https://www.sans.org/security-awareness-training/blog/security-awareness-topic-6-passwords).

As luck would have it, you have some anonymized [datasets](https://github.com/vgutta/CTF-s/tree/master/square/c5/datasets) lying around.

### Solution:

The [Login page](https://glacial-coast-79626.squarectf.com/4WzKpfyFbgdEzO3ONxDPpIXdo9Qps5) has Reset Password button, click on that \
That will bring you to a [Reset Password](https://glacial-coast-79626.squarectf.com/4WzKpfyFbgdEzO3ONxDPpIXdo9Qps5/forgot) page

![Login Page](./Images/Login.jpg)

To reset the password we need to fill in all the fields below

![Reset Page](./Images/Reset.jpg)


The only things we know is name and title.
Searching the name returns
```
$ grep "yakubovics" 1.csv 2.csv 3.csv 4.csv 5.csv
1.csv:eyakubovics9t@nih.gov,Captain,96605
2.csv:eyakubovics9t@nih.gov,Florida
```
That gives us the **email address**

If you open 1.csv you find the 3rd column is the income
So lets search all the files for Captain's income(96605)
```
$ grep "96605" 1.csv 2.csv 3.csv 4.csv 5.csv
1.csv:eyakubovics9t@nih.gov,Captain,96605
4.csv:96605,Florida,4 Magdeline,33421
```
That gives us **street**

```
$ grep "4 Magdeline" 1.csv 2.csv 3.csv 4.csv 5.csv
3.csv:4484,Florida,4 Magdeline
4.csv:96605,Florida,4 Magdeline,33421
5.csv:Elyssa,4 Magdeline
```
3.csv has SSN's, state, and street \
Since there's only one result for that street in 3.csv we can assume the Captain's **SSN is 4484**

5.csv has Firstname and street \
Captain's **First Name is Elyssa** since there is only one result in 5.csv

We know Elyssa is her firtname so Yakubovics has to be her **Last Name**

![Reset Fields](./Images/ResetFields.jpg)

Submitting with those values brings us to the reset password page where the first text box is populated with her current password

![Success](./Images/success.jpg)

Inspecting the HTML of the first text box reveals the **flag**

![Reveal](./Images/Reveal.jpg)
