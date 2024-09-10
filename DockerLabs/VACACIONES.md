<h1 align="center">Vacaciones</h1>

## 0. HERRAMIENTAS

- Hydra
- Binario perl

## 1. ENUMERACIÓN

Realizamos un escaneo de puertos

```jsx
sudo nmap -p- --open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 172.17.0.2
```

Vemos los puertos 22 y 80 abiertos

![image](https://github.com/user-attachments/assets/c4f603de-176f-408a-bd22-5a11347263b3)


Vemos que el index de la web está en blanco

![image 1](https://github.com/user-attachments/assets/6856c2af-f4a2-46cf-aa23-11ec70ebdfd1)


Pero consultando el código fuente vemos lo siguiente

![image 2](https://github.com/user-attachments/assets/acf5b026-064c-4e00-81a1-785e7289b575)


Parece parte de un correo electrónico.

Encontramos dos posibles usuarios:

- juan
- camilo

Realizamos un escaneo de directorios web, pero no encontramos nada más

![image 3](https://github.com/user-attachments/assets/bf36f6d9-b7c5-4bf1-94f2-23ee27c0c5ff)


## 2. COMPROMISO DEL ENTORNO

Crearemos un archivo users con los 2 nombres que hemos encontrado antes, para poder realizar un ataque de fuerza bruta con Hydra

![image 4](https://github.com/user-attachments/assets/32047535-7351-42b2-9c5c-7977b365c268)


```jsx
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10 
```

![image 5](https://github.com/user-attachments/assets/e4e932a4-88a0-487b-854b-f599bb348f5b)


Encontramos la contraseña del usuario camilo

Procederemos a acceder por ssh con ese usuario y contraseña


![image 6](https://github.com/user-attachments/assets/6ca75cce-45df-47f0-94a0-d1721852396c)

 

## 3. ELEVACION DE PRIVILEGIOS

No tiene sudo -l 

![image 7](https://github.com/user-attachments/assets/89f35763-f93e-4386-9003-f9d07c4557ea)


Recordamos anteriormente el fragmento de email. Probamos a buscar mails almacenados

Camilo tiene un correo de juan donde vemos que le manda la contraseña de usuario

![image 8](https://github.com/user-attachments/assets/af089e68-c160-43b6-8e5b-b9aaa3f529de)


Conseguimos acceder correctamente con el usuario de juan

![image 9](https://github.com/user-attachments/assets/6f2239ed-bd38-46eb-9fc1-0ace61545ded)


El usuario juan si que tiene acceso sudo -l

![image 10](https://github.com/user-attachments/assets/fd05dbad-598d-42c5-9b39-8c214c262e8a)


Buscamos el binario ruby en gtfobins y encontramos que nos deja elevar privilegios

![image 11](https://github.com/user-attachments/assets/10b62ade-69ea-4819-933e-c5beab0c807c)

![image 12](https://github.com/user-attachments/assets/d08ec2b9-cb02-46a2-aed8-297b892773ed)

