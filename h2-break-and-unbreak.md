# h2 Break & Unbreak

## x) Read/watch/listen and summarize.

### OWASP: OWASP Top 10: A01 Broken Access Control

* Broken access control in a nutshell means that an user gets access/can do to stuff they're not supposed to, i.e. a customer finds access to a admin dashboard to mess around with

* Preventing broken access control comes down to enforcing proper roles and making sure you don't leave anything important to be found by people who don't have acces to it. Deny deny deny!

### Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf

* *ffuf* is a tool that uses fuzzing to find hidden directories, header, POST parameters etc. easily instead of manually trying to find them.

* Fuzzing basically spams an application with different unexpected inputs. Read more [here](https://owasp.org/www-community/Fuzzing)!

### PortSwigger: Access control vulnerabilities and privilege escalation

* Examples of access control vulnerabilities could be:
  * **Vertical privilege escalation**: User gains access to something that's not meant for them from a more privildeged user, like a admin dashboard.
    * **Unprotected functionality**: User finds an url they're not meant to have access to, i.e. `https://website.io/delete_everything_permanently_forever`
    * **Parameter-based access control methods**: User manipulates values, like cookies or fields, and get's access to something they shouldn't, i.e. `https://website.io/admin=false` => `https://website.io/admin=true`

  * **Horizontal privilege escalation**: User gains access to something that's not meant from them who got the same priviledges as them, i.e. me gaining access to your (you reading this right now) GitHub or Moodle account.

### Karvinen 2006: Report Writing (in Finnish)

This article by Tero describes some dos and don’ts when writing a report.

***Do***s:
* **Repeatable**: Someone else could get the same exact answers/findings/results as you if they follow your report and use the same enviroment
* **Precise**: When writing your report, give out as many details as you can (don't go over the top)
* **Easy to read/follow**: Use subheading, write grammatically correct etc.
* **Cite your sources**: Shows you've done your research.

***Don't***s:
* **Lying**: Don't claim to have done something you actually haven't done.
* **Plagiarism**: Don't copy/cite someone elses work without citing your sources.
* **Copyright infringement**: It's plagiarism and sometimes even illegal.


## a) Break into 010-staff-only

### Part 1: The prerquisites

To start up this exercise I'm booting up my VM that has Debian 13 installed on it:

![h2-a-01](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-01.png)

Check that beast of a VM.

Anyways now let's install the required software by running these commands:
```
sudo apt-get update
sudo apt-get -y install wget unzip micro
```

![h2-a-02](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-02.png)

Wonderful. Now let's download the required files for the exercise. Because I'm running linux and using boxes, I can just download the zip on my actual pc and drag and drop the files to the VM. 

![h2-a-03](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-03.png)

![h2-a-04](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-04.png)

Boom. As you can see the zip is now on my Desktop. I'll move it to my Documents folder and unzip it there using these commands:

```
sudo mv teros-challenges.zip ~/Documents/
cd ~/Documents/
unzip teros-challenges.zip
```

![h2-a-05](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-05.png)

Tadah we're done. Now let's move on to the actual exercise.


### Part 2: The challenge

We'll navigate to challenge and install the required python packages to actually run the challenge like so:

```
sudo apt-get -y install python3-flask python3-flask-sqlalchemy
```

and now we can (most likely) run the challenge:

![h2-a-06](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-06.png)

Wahoo. Since the challenge is running locally I'll disconnect from the internet just in case. I do this by clicking the GUI since I'm too lazy to find out if there's a command that does that.

Now let's open a web browser and navigate to the challenge.

![h2-a-07](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-07.png)

Now let's try to hack this thing! Our only piece of hint the game advice at the bottom of the page:

> **Game advice**
>
> Your pin code is 123. Can you reveal admin password trough the web user interface?
>
> Vulnerable Super Secure Password Recover. Copyright 2018–2024 Tero Karvinen TeroKarvinen.com. Normally targets don't give you this sql:  
> `SELECT password FROM pins WHERE pin='0';`

When I type my pin `123`, the website tells me that my password is *Somedude*. If I type in anything else, such as `000`, the website saysthat my password is (not found).
One way I could do this is forcefeed the input field with numbers until something else shows up, but that'd be tedious. 

Typing anything else but numbers gives us an error: 

![h2-a-08](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-08.png)

So trying the SQL injection thing we did at class last time (`' OR 1=1--`) is out of the picture, atleast for now.

Let's try inspecting the page and the form to see what we can do with that.
I'll right click the form and inspect it.

![h2-a-09](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-09.png)

We find out that the input type is `number`. If we change that to `text` we can input text 🤯. Crazy.

Let's switch `<input type="number" name="pin" value='123'>` to `<input type="text" name="pin" value='hello'>`

And press the reveal my password button:

![h2-a-10](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-10.png)

The password is (not found) again, but it let us sumbit the query. We're on the right track probably.

Let's try to submit this as the query: `123 ' OR 1=1--`

![h2-a-11](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-11.png)

That went through, and the password is *foo*. That does not include the magic word *SUPERADMIN* So this isn't the correct solution. Neat. What now?

Honestly, I got nothing. Time to go peep at the hints.
I'll try the **Tips for 010 - Staff only** at first, I won't jump into the spoilers yet.

I feel like I got everything from the *Never trust the client* section, I've modified the form and managed to put in values that shouldn't be possible. In the *SQL Injection* section one thing that got my attention is the *LIMIT*. My SQL is a little rusty since I took a class on it like 2+ years back and haven't really used it since, but since it's in all caps like the *SELECT* it must be something you do with SQL. Let's research that lead.

Searching *limit sql* brings up a w3schools page called *MySQL LIMIT Clause*. Here's what the page tells us about the *LIMIT*:

>The LIMIT clause is used to specify the number of records to return.
>
>The LIMIT clause is useful on large tables with thousands of records. Returning a large number of records can impact performance.

Readin the entire article it seems like I can limit the amount of items returned by a query, and by using *OFFSET* I can choose where the query starts on the list. 

Now to figure out how to add those to the query on the website.

I'll try out `<input type="text" name="pin" value='123 ' OR 1=1 LIMIT 1 --'>` first.

![h2-a-12](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-12.png)

Gave me the same exact answer as before. Let's try limit 2 if that does anything at all:

![h2-a-13](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-13.png)

Still foo. Let's try *OFFSET* as well. So our query would be `<input type="text" name="pin" value='123 ' OR 1=1 LIMIT 1 OFFSET 1 --'>` = `SELECT password FROM pins WHERE pin='123 ' OR 1=1 LIMIT 1 OFFSET 1 --';` From my understanding (very limited) this would return the next entry on the table after *foo*. I hope.

![h2-a-14](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-14.png)

We've arrived to Somedude. Progress? Let's try to bumb the *OFFSET* to 2 and see what happens. 
`SELECT password FROM pins WHERE pin='123 ' OR 1=1 LIMIT 1 OFFSET 2 --';` time: 

![h2-a-15](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-a-15.png)

Finally! We got the password *SUPERADMIN%%rootALL-FLAG{Tero-e45f8764675e4463db969473b6d0fcdd}*. Now to figure out how to fix this...

## b) Fix the 010-staff-only vulnerability from source code.

Ok since the thing that's enabling us to send malicious queries is that the validation is happening at clientside (the webapp) insteadof the server (the python app). My python is a little rusty but we ball.

One thing that immediately catches my eye in the code is this part:

```
@app.route("/", methods=['POST', 'GET'])
def hello():
	if "pin" in request.form:
		pin = str(request.form['pin'])
	else:
		pin = "0"
```

The `pin = str`. We want numbers for the pin. Let's change that to `pin = int` and see if that fixes it.

I `nano staff-only.py` in the folder and do the changes above. Now let's boot up this badboy and test it out.

![h2-b-01](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-b-01.png)

Yeah this badboy gave me a Internal Server Error and a error message in the console:

```
[2026-01-25 02:02:03,604] ERROR in app: Exception on / [POST]
Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/flask/app.py", line 1511, in wsgi_app
    response = self.full_dispatch_request()
  File "/usr/lib/python3/dist-packages/flask/app.py", line 919, in full_dispatch_request
    rv = self.handle_user_exception(e)
  File "/usr/lib/python3/dist-packages/flask/app.py", line 917, in full_dispatch_request
    rv = self.dispatch_request()
  File "/usr/lib/python3/dist-packages/flask/app.py", line 902, in dispatch_request
    return self.ensure_sync(self.view_functions[rule.endpoint])(**view_args)  # type: ignore[no-any-return]
           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^
  File "/home/whatmurder/Documents/challenges/010-staff-only/staff-only.py", line 22, in hello
    sql = "SELECT password FROM pins WHERE pin='"+pin+"';"
          ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~
TypeError: can only concatenate str (not "int") to str
127.0.0.1 - - [25/Jan/2026 02:02:03] "POST / HTTP/1.1" 500 -
```

That error stems from me not reading the entire code properly and noticing that the pin is in *VARCHAR* format which isn't *int*. I tried changing the *VARCHAR* to *INT* but that gave me an error as well.
Back to the drawing board.

Another thing I can try is validating the input some other way. So let's search for how to validate input in python.
I found a site from *geeksforgeeks* called (Input validation in Python)[https://www.geeksforgeeks.org/python/input-validation-in-python/] and the REGEX validation seems to be the one I'm looking for in this usecase. Before I did that thought I searched *can regex stop sql injection* and saw many threads saying hey maybe don't do that? 
Back to the drawing board.


## c) Solve dirfuzt-1

The idea for this exercise is to find a hidden dricetory in the provided sample file **dirfuzt-1**

Let's download that, fuff, and the common.txt:

```
sudo apt-get update
sudo apt-get install ffuf
wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1
chmod u+x dirfuzt-1
wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt
```

Now that that's downloaded let's disable the internet connection and start *fuffing* like crazy to find the two URLS:
* Admin page
* Version control related page

Opening the url we get greeted with a page like this:

![h2-c-01](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-c-01.png)

Let's ffuf around and find out:

We run the command `ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ`

![h2-c-02](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-c-02.png)


We got some usable information: The size of most of these are 154. We can use that in our next ffufs.

Let's filter out the out ones with that size using the command `ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154`:

This query found these pages:
```
.git                    [Status: 301, Size: 41, Words: 3, Lines: 3, Duration: 0ms]
.git/config             [Status: 200, Size: 178, Words: 6, Lines: 11, Duration: 0ms]
.git/HEAD               [Status: 200, Size: 178, Words: 6, Lines: 11, Duration: 0ms]
.git/logs/              [Status: 200, Size: 178, Words: 6, Lines: 11, Duration: 0ms]
.git/index              [Status: 200, Size: 178, Words: 6, Lines: 11, Duration: 0ms]
render/https://www.google.com [Status: 301, Size: 64, Words: 3, Lines: 3, Duration: 0ms]
wp-admin                [Status: 200, Size: 182, Words: 6, Lines: 11, Duration: 0ms]
```

![h2-c-03](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-c-03.png)


Let's check out the page called wp-admin first `http://127.0.0.2:8000/wp-admin`:

![h2-c-04](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-c-04.png)

We got a hit, flag gained! `FLAG{tero-wpadmin-3364c855a2ac87341fc7bcbda955b580}`

Now let's try those .git pages, starting with `http://127.0.0.2:8000/.git/`:

![h2-c-05](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h2-c-05.png)

Another flag gained. `FLAG{tero-git-3cc87212bcd411686a3b9e547d47fc51}`

I went through rest of the .git pages and they had the same flag, so safe to say we caught all the flags. Nice job me.

## d) Break into 020-your-eyes-only

gyugyugyhuo

## e) Fix the 020-your-eyes-only vulnerability.

aasaserdtciftgybuouj

### Sources:

https://terokarvinen.com/application-hacking/

https://owasp.org/Top10/A01_2021-Broken_Access_Control/

https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/

https://owasp.org/www-community/Fuzzing

https://portswigger.net/web-security/access-control

https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

https://terokarvinen.com/hack-n-fix/

https://www.w3schools.com/mysql/mysql_limit.asp

https://www.w3schools.com/sql/sql_datatypes.asp

https://www.geeksforgeeks.org/python/input-validation-in-python/

https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html

https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/

https://github.com/danielmiessler/SecLists

https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

