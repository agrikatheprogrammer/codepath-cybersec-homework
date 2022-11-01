# Pen Testing Live Targets

Time spent: **X** hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:

* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each color is vulnerable to only 2 of the 6 possible exploits. First discover which color has the specific vulnerability, then write a short description of how to exploit it, and finally demonstrate it using screenshots compiled into a GIF.

## Blue

Vulnerability #1: Session Hijacking/Fixation

Description:

<img src="http://g.recordit.co/QT9IzYk3sH.gif">

Vulnerability #2: __________________

Description:

<img src="blue-vuln1.gif">


## Green

Vulnerability #1: Cross-Site Scripting (XSS)

Description: Cross site scripting (XSS) is an attack in which an attacker injects malicious executable scripts into the code of a trusted application or website. Attackers often initiate an XSS attack by sending a malicious link to a user and enticing the user to click it. 

In this case, XSS can be embedded in the feedback/contact form and triggered when admin/user opens the feedback. This is a form of stored XSS.

<img src="http://g.recordit.co/JDWtU0qIJR.gif">

Vulnerability #2: __________________

Description:

<img src="blue-vuln1.gif">


## Red

Vulnerability #1: Insecure Direct Object Reference (IDOR)

Description: Insecure Direct Object References (IDOR) occur when an application provides direct access to objects based on user-supplied input. As a result of this vulnerability attackers can bypass authorization and access resources in the system directly, for example database records or files.

In this case, employees that were removed or are to be listed in the future can be accessed by any user without authorization.

<img src="http://g.recordit.co/JcCzrfQiOt.gif">

Vulnerability #2: _________________

Description:

<img src="blue-vuln1.gif">


