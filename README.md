# Pen Testing Live Targets

Time spent: **7** hours spent in total

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

Description: Session Fixation is an attack that permits an attacker to hijack a valid user session. 

In this case, I logged in using Chrome and the session id was changed to **codepath**. Internet Explorer browser was opened afterward. The session for the Internet Explorer's Blue Target was changed from its previous one (logged out state) to **codepath** too. Despite being not logged in, I was able to hijack the other user's session because I got logged in after copying a valid, existing session id of the Chrome browser.

<img src="http://g.recordit.co/QT9IzYk3sH.gif">

Vulnerability #2: SQL Injection (SQLi)

Description: SQL injection, also known as SQLi, is a common attack vector that uses malicious SQL code for backend database manipulation to access information that was not intended to be displayed. This information may include any number of items, including sensitive company data, user lists or private customer details.

The database can be put to sleep for 5 seconds using a SQL Injection. To do so, all we need to inject is **' OR SLEEP(5)=0--'** into the **id=** section of the url. 

<img src="http://g.recordit.co/Q5u62Bv9QT.gif">


## Green

Vulnerability #1: Cross-Site Scripting (XSS)

Description: Cross site scripting (XSS) is an attack in which an attacker injects malicious executable scripts into the code of a trusted application or website. Attackers often initiate an XSS attack by sending a malicious link to a user and enticing the user to click it. 

In this case, XSS can be embedded in the feedback/contact form and triggered when admin/user opens the feedback. This is a form of stored XSS.

<img src="http://g.recordit.co/JDWtU0qIJR.gif">

Vulnerability #2: Username Enumeration

Description: Username enumeration is a common application vulnerability which occurs when an attacker can determine if usernames are valid or not. Most commonly, this issue occurs on login forms, where an error similar to “the username is invalid” is returned.

When an existing username is typed in with a random password, the words, "Log in was unsuccessful", are shown in bold. Otherwise, the characters are in normal font without any bolding. Thus, one can figure out which usernames are valid and which are not.

<img src="http://g.recordit.co/NYvawyKM8Y.gif">


## Red

Vulnerability #1: Insecure Direct Object Reference (IDOR)

Description: Insecure Direct Object References (IDOR) occur when an application provides direct access to objects based on user-supplied input. As a result of this vulnerability attackers can bypass authorization and access resources in the system directly, for example database records or files.

In this case, employees that were removed or are to be listed in the future can be accessed by any user without authorization.

<img src="http://g.recordit.co/JcCzrfQiOt.gif">

Vulnerability #2: Cross-Site Request Forgery (CSRF)

Description:

<img src="blue-vuln1.gif">


