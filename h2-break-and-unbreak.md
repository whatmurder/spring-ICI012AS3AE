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
    * **Unprotected functionality**: User finds an url they're not meant to have access to, i.e. ```https://website.io/delete_everything_permanently_forever```
    * **Parameter-based access control methods**: User manipulates values, like cookies or fields, and get's access to something they shouldn't, i.e. ```https://website.io/admin=false``` => ```https://website.io/admin=true```

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

bygoyi


## b) Fix the 010-staff-only vulnerability from source code.

bhbibilybb

## c) Solve dirfuzt-1

yuyuyuogyu

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

https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/

https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

