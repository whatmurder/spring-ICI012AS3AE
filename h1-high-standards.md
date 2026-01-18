# High Standards

## a) Baseline

### Home Network Basic Infrastructure

* Router
* Wi-Fi for home devices (Desktop PC, MacBook)
* Guest Wi-Fi for work laptop and the Linux laptop

### Devices Used for Course Exercises

* **Desktop PC** running Arch Linux  
  * Virtual machine within Arch Linux running **Debian 13** for course work  
    * VM is set up using the Flatpak version of **GNOME Boxes**
    * Used at home for course related Linux work.
* **MacBook Air**  
  * Used for coding, reading, browsing, and miscellaneous school work  
  * Used both in class and at home
* **Linux Laptop** running Debian 13  
  * Used for Linux-related work at school  
  * Required since the MacBook can’t run amd64 infrastructure distributions  
  * Used mainly in school for course related Linux work.
* **Phone**  
  * Used for MFA

### Information

* Course page on terokarvinen.com
* Course page on Moodle
* GitHub repository for the course work.
* GitHub account
* Haaga-Helia Microsoft account for school stuff.

### Exclusions

* Work Laptop
  * Work account policies, managed by employer. Not at home all the time.
* SmartTV
   * Irrelevant for coursework. Not connected to the Wi-Fi. Hasn't been on in months.
* Nintendo 3DS
   * Irrelevant for coursework. Not connected to the Wi-Fi.

### Key Interfaces

* GitHub
* Moodle
* ISP (Elisa)
* Phone Provider (DNA)
* VPN (Mullvad)
* Firewall (UFW)
  * ```ufw default deny```

![My Cool Graph](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h1-graph.png)

Here's a pic of Boxes and my VM in it:
![Boxes and VM](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h1-vm.png)

## b) Tie It to the Standard

Not sure if I fully grasped what the exercise was about but here's a table lol:

|   **Interested Party**  | **Need/Expectation/Requirement** | **ISO 27001 Requirement Area** | **Evidence/Demonstration** |
|---------------------|------------------------------|-----------------------------|-----------------------|
| Me (Owner and user) | Continuity of exercises, data retention. | Planning (6.1, 6.2) & Operation (8.1) | Following best practises and following guidelines set by exercises. Snapshots and backups (if necessary) of exercises. Documentation of work. |
| Service providers (GitHub, Moodle) | Compliance with terms, MFA, Account Security | Operation (8.1)| Multifactor authentication on, strong passwords. |

Lil bit of evidence:
![MFA on on Github](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h1-auth.png)


### Sources:

https://terokarvinen.com/application-hacking/

HH Moodle: Application Hacking and Vulnerabilities - ICI012AS3AE-3001

https://en.wikipedia.org/wiki/ISO/IEC_27001

ISO27001-2023-fi.pdf: SFS-EN ISO/IEC 27001:2023

https://www.codecademy.com/resources/docs/markdown/tables