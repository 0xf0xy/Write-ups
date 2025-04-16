<p align="center">
  <img src="https://tryhackme-images.s3.amazonaws.com/room-icons/5e9c5d0148cf664325c8a075-1737130517336" alt="Neighbour" width="200">
</p>

# Neighbour
> ***Check out our new cloud service, Authentication Anywhere. Can you find other user's secrets?***

<br>

---
## 🧠 Intro
This room introduces us to a common web vulnerability called **IDOR (Insecure Direct Object Reference)**. Basically, it happens when a website lets you access data by changing values in the URL (like usernames or IDs), without checking if you're actually allowed to see it. 😅

*So let's deploy de machine and start Hacking!* 😎

<br>

---
## 🔍 Recon
After deploying the machine, we visit the IP in the browser:

http://<MACHINE_IP>

![1](#Neighbour_1.png)

We’re welcomed with a basic login page. Below the form there's a helpful tip:

> *"If you don't have an account, use the guest account (Ctrl+U)"*

So we hit `Ctrl+U` to peek at the HTML source and find this little gem:

```html
<!-- use guest:guest credentials until registration is fixed. "admin" user account is off limits!!!!! -->
Nice. Time to login.
```

🔑 Logging In
We log in with the credentials:

Username: guest

Password: guest

Boom. We're redirected to:

arduino
Copiar
Editar
http://<MACHINE_IP>/profile.php?user=guest
And hey — the user parameter is sitting right there in the URL. Suspicious.

🧪 Exploitation (IDOR FTW)
Let’s test if we can mess with that user value.

So we swap guest with admin:

pgsql
Copiar
Editar
http://<MACHINE_IP>/profile.php?user=admin
And it works! We get access to the admin profile without needing admin creds. That’s classic IDOR in action.

🏁 Got the Flag
On the admin profile page, we find our flag:

Copiar
Editar
flag{66be95c478473d91a5358f2440c7af1f}
Easy win 💥

🎯 Takeaway
This room is a nice reminder that just hiding stuff isn’t security. Always make sure your app checks permissions server-side. Never trust users to only access what they should.

📚 Resources
TryHackMe - Neighbour

PortSwigger - IDOR
