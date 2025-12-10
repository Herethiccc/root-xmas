# Santa Root Kit

![jason](https://github.com/user-attachments/assets/205327fc-c636-457f-92e1-a1fef0d163cd)

## Contexte
> Oh no, it looks like the Grinch’s assistant pwned us !
> Luckily, our experts managed to recover two key informations:
> - A file hash: ```bf6a54d181b9bbcb168720b9d466ca27d14ead283220ee5a9e777352c16edca3```
> - A leaked message that said more intels can be found somewhere in a vault.
>
> Our team keeps digging but we wouldn’t mind a bit of help..
> Will you please help us finding intel on them ?


## Step 1
We are provided with a hash, let’s head to VirusTotal and see if we manage to find some infos in order to start our investigation.<br>
Uploading our hash, we can see it was already analyzed before, probably the Grinch’s assistant doing some early test.<br></br>
At first glance, we barely have anything interesting, except for the file’s name:
<img width="505" height="65" alt="vt" src="https://github.com/user-attachments/assets/7c5114b2-e2fe-4967-82f2-95aad2ec356a" />

## Step 2
What if they never renamed that file ? Let’s check the usual platforms.<br></br>
Github got a hit, there’s a very suspicious looking dog who seems to have pushed a few commits that could match our case, and the repository’s name itself is suspicious as well:
<img width="759" height="307" alt="gh1" src="https://github.com/user-attachments/assets/582d7226-bc31-49f6-a6cd-85ac0a6bc2c7" />
<img width="1344" height="510" alt="gh2" src="https://github.com/user-attachments/assets/e5b5dd10-1886-4765-9a32-bd7a9b6bf9d1" /><br></br>
**Looking at that repository, we might be on the right track** 👀
<img width="1495" height="912" alt="gh3" src="https://github.com/user-attachments/assets/190b7361-e15e-4e05-b01a-77c353367cc4" /><br></br>
So, going through the repository and it’s commits, there are a couple things that catch our attention.
- First of all, we found an email address by checking the patch files associated to the commits (simply append .patch at the end of the url)<br></br>
<img width="547" height="66" alt="commit" src="https://github.com/user-attachments/assets/6f6d31ef-89d1-4a27-826a-b8b04b5f44a4" />

[Example with commit 57019a86192eb4e9d147443460bf4aba76d39a76](https://github.com/jbashdapuppet/santa-root-kit/commit/57019a86192eb4e9d147443460bf4aba76d39a76.patch)
- We also found an interesting #TODO in the sleighctl.py file:<br>
```#TODO: Optimize the code / Add comments / Replace our PGP key on "that website"..```<br></br>

It doesn’t seem like there is much more to get from that GitHub tho..
The email is too recent, so it doesn’t give results either.

## Step 3
**What about that PGP key ?**<br>
PGP keys are used in quite a few ways.
Let’s see what the web says about PGP in an Osint context.<br></br>

A quick search suggests this [article](https://nixintel.info/osint-tools/using-pgp-keys-for-osint/) from Nixintel:

- The first and second tools showcased are interesting but won’t be of much help since we don’t have any key so far.
- But the third one is about [Keybase](https://keybase.io/) , a PGP oriented social media.

Searching for the email address we found isn’t useful:<br>
<img width="445" height="91" alt="keybase1" src="https://github.com/user-attachments/assets/021ef3a5-23a0-468d-be0e-71ce807d035a" />

But what if we went for the nickname instead ?<br>
<img width="465" height="111" alt="keybase2" src="https://github.com/user-attachments/assets/f6a3185f-4598-4a49-9065-c65221b11d00" /><br>
The first result seems to be a legitimate user but the second one rings a bell, it uses the same profile picture that was used by the Github account.<br></br>

<img width="716" height="295" alt="keybase3" src="https://github.com/user-attachments/assets/dab007a9-05c8-4a9b-a0a2-c92f29e182bc" /><br>
Bingo ! Their bio is also the same sentence that was in the Github repository’s readme file.<br></br>

Going around the profile, we can see some activity, a few registered devices etc.
They also added a PGP fingerprint to their profile.<br></br>

Keybase lets you download a user’s PGP keys, so let’s give that a try:
<img width="799" height="328" alt="keybase4" src="https://github.com/user-attachments/assets/76e43fa4-aef2-450e-a79c-41020f1305a1" />
<img width="600" height="574" alt="keaybase_pgp_keys" src="https://github.com/user-attachments/assets/6d0d6140-e97d-4a3f-86d9-87813ef8e7cb" /><br></br>

Let’s grab the key and see if we can get some infos out of it.<br></br>

By running a simple ```gpg <file>``` command, we can see the dummy has added way too many infos in the comment, leaving a Proton Drive URL:
<img width="1400" height="126" alt="pgp_comment" src="https://github.com/user-attachments/assets/88ff5de0-d690-4864-afb3-5f8329fe339c" /><br>

## Step 4
Accessing that URL leads us to the following Vault:<br>
<img width="1843" height="263" alt="vault1" src="https://github.com/user-attachments/assets/cc0ab934-682e-4c38-a45f-31c59a957f45" />

It seems like it does indeed contain the “new key” and some other materials that should definitely not be in there:
<img width="1844" height="296" alt="vault2" src="https://github.com/user-attachments/assets/65794ba4-0c3d-4b70-9bb4-36af5063db55" /><br></br>

And apparently, they even managed to fail the sharing process, they added a link to the wrong folder, providing access to the Santa's naughty secrets folder.<br></br>

And here is our flag :shipit:
<img width="1849" height="294" alt="vault3" src="https://github.com/user-attachments/assets/6b59f2ce-0c0f-45aa-bfa3-807057824ad4" /><br></br>

```FLAG: RM{H0_H0_H0_1_M16H7_83_1N_7r0U813}```
