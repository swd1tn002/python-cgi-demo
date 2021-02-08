# Python CGI demo on Ubuntu 20.04

## Install Apache

```
$ sudo apt install apache2
$ sudo systemctl is-enabled apache2.service
```

### Configure mod_cgid

```
$ sudo a2enmod cgid
```

### Configure Apache2

```
$ sudo nano /etc/apache2/apache2.conf
```

Add ExecCGI and AddHandler parts in the `/var/www/` directory configuration:

```
<Directory /var/www/>
        AllowOverride None
        Options Indexes FollowSymLinks ExecCGI
        AddHandler cgi-script .py .cgi
        Require all granted
</Directory>
```

### Restart Apache

```
sudo systemctl restart apache2.service
```



## Write Python file as a CGI script

```
$ sudo nano /var/www/html/index.py
```

Write your Python module:

```python
#!/usr/bin/python3
import sys
import json
import cgitb
cgitb.enable()

print('Content-Type: text/html\n\n')

# Print the request body:
for line in sys.stdin:
    print(line)

print('Done')
```


### Allow execution

```
$ sudo chmod +x /var/www/html/index.py
```

### Try it out

Post a test file with curl and verify results:

```
$ curl -d "@testfile.txt" -X POST http://localhost/index.py
```
