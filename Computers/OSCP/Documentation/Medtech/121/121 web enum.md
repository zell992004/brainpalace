

## Web Directories Discovered

[Status: 301, Size: 156, Words: 9, Lines: 2, Duration: 364ms]
    * FUZZ: assets
* [Status: 301, Size: 153, Words: 9, Lines: 2, Duration: 276ms]
    * FUZZ: css
* [Status: 301, Size: 155, Words: 9, Lines: 2, Duration: 179ms]
    * FUZZ: fonts
[Status: 301, Size: 152, Words: 9, Lines: 2, Duration: 141ms]
    * FUZZ: js
[Status: 301, Size: 156, Words: 9, Lines: 2, Duration: 212ms]
    * FUZZ: master

Hmm.. from running this before I know there is a login page that seems to be missing...
running
```bash
└─$ dirb http://192.168.200.121 /usr/share/dirb/wordlists/common.txt
```

trying seclist med 

```bash
dirb http://192.168.200.121/ /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt 
```

Login page found while viewing the site at https://192.168.200.121/login.aspx As it was found that this was a Microsoft-IIS web server, there could be potential sql injection.

gobuster dir -u http://192.168.200.121 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x .aspx -t 10
===============================================================
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.200.121
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Extensions:              aspx
[+] Timeout:                 10s
===============================================================
2023/07/16 16:10:38 Starting gobuster in directory enumeration mode
===============================================================
/about.aspx           (Status: 200) [Size: 22575]
/contact.aspx         (Status: 200) [Size: 46706]
/blog.aspx            (Status: 200) [Size: 30752]
/default.aspx         (Status: 200) [Size: 25997]
/login.aspx           (Status: 200) [Size: 4083]
/services.aspx        (Status: 200) [Size: 19928]
/Default.aspx         (Status: 200) [Size: 25997]
/assets               (Status: 301) [Size: 153] [--> http://192.168.200.121/assets/]
/css                  (Status: 301) [Size: 150] [--> http://192.168.200.121/css/]
/Contact.aspx         (Status: 200) [Size: 46706]
/About.aspx           (Status: 200) [Size: 22575]
/Login.aspx           (Status: 200) [Size: 4083]
/js                   (Status: 301) [Size: 149] [--> http://192.168.200.121/js/]
/Blog.aspx            (Status: 200) [Size: 30752]
/Services.aspx        (Status: 200) [Size: 19928]
/master               (Status: 301) [Size: 153] [--> http://192.168.200.121/master/]
/error.aspx           (Status: 200) [Size: 962]
/fonts                (Status: 301) [Size: 152] [--> http://192.168.200.121/fonts/]
/Assets               (Status: 301) [Size: 153] [--> http://192.168.200.121/Assets/]
/Fonts                (Status: 301) [Size: 152] [--> http://192.168.200.121/Fonts/]
/*checkout*           (Status: 400) [Size: 3490]
/*checkout*.aspx      (Status: 400) [Size: 3490]
/CSS                  (Status: 301) [Size: 150] [--> http://192.168.200.121/CSS/]
/JS                   (Status: 301) [Size: 149] [--> http://192.168.200.121/JS/]
/*docroot*            (Status: 400) [Size: 3490]
/*docroot*.aspx       (Status: 400) [Size: 3490]
/*.aspx               (Status: 400) [Size: 3490]
/*                    (Status: 400) [Size: 3490]
/http%3A%2F%2Fwww.aspx (Status: 400) [Size: 3490]
/http%3A%2F%2Fwww     (Status: 400) [Size: 3490]
/CONTACT.aspx         (Status: 200) [Size: 46706]
/Error.aspx           (Status: 200) [Size: 962]
/Master               (Status: 301) [Size: 153] [--> http://192.168.200.121/Master/]
/http%3A              (Status: 400) [Size: 3490]
/http%3A.aspx         (Status: 400) [Size: 3490]
/q%26a                (Status: 400) [Size: 3490]
/q%26a.aspx           (Status: 400) [Size: 3490]
/**http%3a.aspx       (Status: 400) [Size: 3490]
/**http%3a            (Status: 400) [Size: 3490]
/*http%3A.aspx        (Status: 400) [Size: 3490]
/*http%3A             (Status: 400) [Size: 3490]
/MASTER               (Status: 301) [Size: 153] [--> http://192.168.200.121/MASTER/]
/SERVICES.aspx        (Status: 200) [Size: 19928]
/ABOUT.aspx           (Status: 200) [Size: 22575]
/**http%3A.aspx       (Status: 400) [Size: 3490]
/**http%3A            (Status: 400) [Size: 3490]
/http%3A%2F%2Fyoutube.aspx (Status: 400) [Size: 3490]
/http%3A%2F%2Fyoutube (Status: 400) [Size: 3490]
/http%3A%2F%2Fblogs   (Status: 400) [Size: 3490]
/http%3A%2F%2Fblogs.aspx (Status: 400) [Size: 3490]
/http%3A%2F%2Fblog.aspx (Status: 400) [Size: 3490]
/http%3A%2F%2Fblog    (Status: 400) [Size: 3490]
/**http%3A%2F%2Fwww   (Status: 400) [Size: 3490]
/**http%3A%2F%2Fwww.aspx (Status: 400) [Size: 3490]
/DEFAULT.aspx         (Status: 200) [Size: 25997]
/s%26p                (Status: 400) [Size: 3490]
/s%26p.aspx           (Status: 400) [Size: 3490]
/LogIn.aspx           (Status: 200) [Size: 4083]
/%3FRID%3D2671        (Status: 400) [Size: 3490]
/%3FRID%3D2671.aspx   (Status: 400) [Size: 3490]
/devinmoore*.aspx     (Status: 400) [Size: 3490]
/devinmoore*          (Status: 400) [Size: 3490]
/LOGIN.aspx           (Status: 200) [Size: 4083]
/200109*.aspx         (Status: 400) [Size: 3490]
/200109*              (Status: 400) [Size: 3490]
/*sa_.aspx            (Status: 400) [Size: 3490]
/*sa_                 (Status: 400) [Size: 3490]
/*dc_                 (Status: 400) [Size: 3490]
/*dc_.aspx            (Status: 400) [Size: 3490]
/http%3A%2F%2Fcommunity (Status: 400) [Size: 3490]
/http%3A%2F%2Fcommunity.aspx (Status: 400) [Size: 3490]
/Chamillionaire%20%26%20Paul%20Wall-%20Get%20Ya%20Mind%20Correct (Status: 400) [Size: 3490]
/Chamillionaire%20%26%20Paul%20Wall-%20Get%20Ya%20Mind%20Correct.aspx (Status: 400) [Size: 3490]
/Clinton%20Sparks%20%26%20Diddy%20-%20Dont%20Call%20It%20A%20Comeback%28RuZtY%29 (Status: 400) [Size: 3490]
/Clinton%20Sparks%20%26%20Diddy%20-%20Dont%20Call%20It%20A%20Comeback%28RuZtY%29.aspx (Status: 400) [Size: 3490]
/DJ%20Haze%20%26%20The%20Game%20-%20New%20Blood%20Series%20Pt (Status: 400) [Size: 3490]
/DJ%20Haze%20%26%20The%20Game%20-%20New%20Blood%20Series%20Pt.aspx (Status: 400) [Size: 3490]
/http%3A%2F%2Fradar   (Status: 400) [Size: 3490]
/http%3A%2F%2Fradar.aspx (Status: 400) [Size: 3490]
/q%26a2               (Status: 400) [Size: 3490]
/q%26a2.aspx          (Status: 400) [Size: 3490]
/login%3f             (Status: 400) [Size: 3490]
/login%3f.aspx        (Status: 400) [Size: 3490]
/Shakira%20Oral%20Fixation%201%20%26%202.aspx (Status: 400) [Size: 3490]
/Shakira%20Oral%20Fixation%201%20%26%202 (Status: 400) [Size: 3490]
/%22julie%20roehm%22.aspx (Status: 500) [Size: 3490]
/%22britney%20spears%22.aspx (Status: 500) [Size: 3490]
/%22james%20kim%22.aspx (Status: 500) [Size: 3490]
/http%3A%2F%2Fjeremiahgrossman (Status: 400) [Size: 3490]
/http%3A%2F%2Fjeremiahgrossman.aspx (Status: 400) [Size: 3490]
/http%3A%2F%2Fweblog  (Status: 400) [Size: 3490]
/http%3A%2F%2Fweblog.aspx (Status: 400) [Size: 3490]
/http%3A%2F%2Fswik    (Status: 400) [Size: 3490]
/http%3A%2F%2Fswik.aspx (Status: 400) [Size: 3490]
Progress: 441044 / 441122 (99.98%)


