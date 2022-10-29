# Project 7 - WordPress Pen Testing

Time spent: **7** hours spent in total

> Objective: Find, analyze, recreate, and document **five vulnerabilities** affecting an old version of WordPress

## Pen Testing Report

### 1. Authenticated Stored XSS via Theme Upload

- [x] Summary: XSS Stored because filename of theme when broken, So when theme is broken, Wordpress will inform the name of theme who has been broken which is the folder name of theme and inform the error with description message.
  - Vulnerability types: XSS will be execute , because the filename is stored on page without any filter, and this is possible to make stored XSS. It'll be good to filter / encoding the illegal character, like wordpress do on themes upload.
  - Tested in version: 4.2
  - Fixed in version: 5.4.2
- GIF Walkthrough: <img src="http://g.recordit.co/76wzxcVwmW.gif" width=200><br>
- Steps to recreate: 
1. Upload theme
2. Delete the style.css ( or you can make new folder on theme path with payload name )
3. Rename the folder to script "img src=x onerror=alert(1)"
4. Refresh and see theme page
- Affected source code:
[Link 1](https://core.trac.wordpress.org/browser/tags/version/src/source_file.php)
 https://wpscan.com/vulnerability/d8addb42-e70b-4439-b828-fd0697e5d9d4
 https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-4049
 https://www.exploit-db.com/exploits/48770/
 https://wordpress.org/news/2020/06/wordpress-5-4-2-security-and-maintenance-release/
 https://github.com/WordPress/wordpress-develop/security/advisories/GHSA-87h4-phjv-rm6p
 https://hackerone.com/reports/406289

  
### 2. Unauthenticated file uploads leading to XSS.

- [x] Summary: WordPress Plugin ReFlex Gallery is prone to a vulnerability that lets attackers upload arbitrary files because the application fails to properly sanitize user-supplied input. An attacker can exploit this vulnerability to upload arbitrary code and run it in the context of the webserver process. This may facilitate unauthorized access or privilege escalation; other attacks are also possible. 
This particular vulnerability could allow a user with the ‘upload_files’ capability (Authors and above in a default installation) to upload a file with the filename set to malicious JavaScript, which might be executed when viewing the file in the media gallery
  - Vulnerability types: XSS/CSRF
  - Tested in version: 4.2
  - Fixed in version: 3.4
-  GIF Walkthrough: <img src="http://g.recordit.co/JDBmWQHoJ6.gif" width=200><br>
- [x] Steps to recreate: Use HTML code to create POST for file upload
method="POST" action="http://127.0.0.1:1337/wordpress/wp-content/plugins/reflex-gallery/admin/scripts/FileUploader/php.php?Year=2015&Month=03" enctype="multipart/form-data" >
    <input type="file" name="qqfile"><br>
    <input type="submit" name="Submit" value="Pwn!">
- [ ] Affected source code:
  - [Link 1](https://core.trac.wordpress.org/browser/tags/version/src/source_file.php)

### 3. Stored XSS via Comment Editing

- [x] Summary: WordPress does not properly escape comments, which could lead to Stored Cross-Site Scripting issues.
In most cases, XSS allows an attacker to access victims’ cookies and use them to take over the victim’s session on the server. This can lead to complete account takeover and information theft — including email address, IP address and even credit card data.

Because XSS attacks target users rather than servers or network infrastructure, they’re often overlooked by developers in favor of more complex exploits that require more technical expertise to pull off.
  - Vulnerability types: XSS/CSRF
  - Tested in version: 4.2
  - Fixed in version: 4.2.23
-  GIF Walkthrough: <img src="http://g.recordit.co/u7nAfj9LUf.gif" width=200><br>
- [ ] Steps to recreate: Add XSS scripts to comments!
I used: "svg onload=alert(1)"AND"body onload=window.location='https://codepath.org/'"
- [ ] Affected source code:
  - [Link 1](https://core.trac.wordpress.org/browser/tags/version/src/source_file.php)

### 4. CSRF through post itself

- [ ] Summary: WordPress does not escape captions.
In most cases, XSS allows an attacker to access victims’ cookies and use them to take over the victim’s session on the server. This can lead to complete account takeover and information theft — including email address, IP address and even credit card data.
  - Vulnerability types: CSRF
  - Tested in version: 4.2
  - Fixed in version: 4.2.23
- GIF Walkthrough: <img src="http://g.recordit.co/LsqLd9mG0u.gif" width=200><br>
- [ ] Steps to recreate: Add code/XSS script in caption /post!
span style="font-weight: 400;",click here &lt;script&gt;alert(document.cookie)&lt;/script&gt;</span
- [ ] Affected source code:
  - [Link 1](https://core.trac.wordpress.org/browser/tags/version/src/source_file.php)

### 5. User Enumeration (CVE-2020-7918)

- [x] Summary: ounts, and in scenarios where a person tried to login with default credentials or any username or password the error is specific and provides details on whether username or password right or wrong
  - Vulnerability types:
  - Tested in version: 4.2
  - Fixed in version: 4.5
- GIF Walkthrough: 

<img src="http://g.recordit.co/MDOz41dFKm.gif" width=200><br>


<img src="http://g.recordit.co/DsdYuLhmJ4.gif" width=200><br>
-  Steps to recreate: Here, I made a account in our wordpress, when a person tried to login and tried several username and at last he found that user1 username exist
Using wpscan and a file of many passwords, the username and password was cracked
- [ ] Affected source code:
  - [Link 1](https://core.trac.wordpress.org/browser/tags/version/src/source_file.php) 

## Assets

List any additional assets, such as scripts or files


## Resources

- [WordPress Source Browser](https://core.trac.wordpress.org/browser/)
- [WordPress Developer Reference](https://developer.wordpress.org/reference/)

GIFs created with  ...
<!-- Recommended GIF Tools:
[Kap](https://getkap.co/) for macOS
[ScreenToGif](https://www.screentogif.com/) for Windows
[peek](https://github.com/phw/peek) for Linux. -->


## License

    Copyright [yyyy] [name of copyright owner]

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
