# VACACIONES

## 0. HERRAMIENTAS

- Hydra
- Binario perl

## 1. ENUMERACIÓN

Realizamos un escaneo de puertos

```jsx
sudo nmap -p- --open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 172.17.0.2
```

Vemos los puertos 22 y 80 abiertos

![image.png](image.png)

Vemos que el index de la web está en blanco

![image.png](image%201.png)

Pero consultando el código fuente vemos lo siguiente

![image.png](image%202.png)

Parece parte de un correo electrónico.

Encontramos dos posibles usuarios:

- juan
- camilo

Realizamos un escaneo de directorios web, pero no encontramos nada más

![image.png](image%203.png)

## 2. COMPROMISO DEL ENTORNO

Crearemos un archivo users con los 2 nombres que hemos encontrado antes, para poder realizar un ataque de fuerza bruta con Hydra

![image.png](image%204.png)

```jsx
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10 
```

![image.png](image%205.png)

Encontramos la contraseña del usuario camilo

Procederemos a acceder por ssh con ese usuario y contraseña

![image.png](image%206.png)

 

## 3. ELEVACION DE PRIVILEGIOS

No tiene sudo -l 

![image.png](image%207.png)

Recordamos anteriormente el fragmento de email. Probamos a buscar mails almacenados

Camilo tiene un correo de juan donde vemos que le manda la contraseña de usuario

![image.png](image%208.png)

Conseguimos acceder correctamente con el usuario de juan

![image.png](image%209.png)

El usuario juan si que tiene acceso sudo -l

![image.png](image%2010.png)

Buscamos el binario ruby en gtfobins y encontramos que nos deja elevar privilegios

![image.png](image%2011.png)

![image.png](image%2012.png)