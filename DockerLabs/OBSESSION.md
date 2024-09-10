<h1 align="center">Obsession</h1>
## 0. HERRAMIENTAS

- Hydra
- Binario VIM

## 1. ENUMERACION

Realizamos un escaneo de puertos

```jsx
sudo nmap -p- --open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 172.17.0.2
```

Vemos los puertos abiertos:

- 21
- 22
- 80

![image](https://github.com/user-attachments/assets/f3d37c0d-063d-4cf2-b88e-55c55d520f15)

![image 1](https://github.com/user-attachments/assets/5e28f8a6-ecc1-40b6-ac88-465f60d41b05)


La url no parece tener nada, en cambio vemos en el código fuente la siguiente línea

![image 2](https://github.com/user-attachments/assets/c95c768d-2a03-4516-a389-314efed7a186)

Hacemos un escaneo de directorios y archivos

```jsx
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '[http://172.17.0.2](http://172.17.0.2/)' -x 'html,txt,php,py
```

Vemos los siguientes directorios

![image 3](https://github.com/user-attachments/assets/94a16cc3-2330-485c-82c6-4f78c1be576b)


en /important tenemos el siguiente archivo

![image 4](https://github.com/user-attachments/assets/a6c67f39-d861-490f-b554-be86b9138521)

![image 5](https://github.com/user-attachments/assets/00aa246e-8d33-4082-b73b-367d822d8690)


En backup encontramos lo siguiente, donde nos confirma un usuario “russoski”

![image 6](https://github.com/user-attachments/assets/d0c18edc-b9a5-4046-96b8-4253a8e89d1a)

![image 7](https://github.com/user-attachments/assets/276a6493-6cf1-4297-a163-9d1a94f8967a)


Para ver si encontramos algo más. Probaremos acceso a través del ftp, puerto 21, ya que se puede acceder con sesión anónima.

Vemos que tenemos 2 archivos

![image 8](https://github.com/user-attachments/assets/7f7e86f9-9aee-4374-a799-b286f83a2d0f)

![image 9](https://github.com/user-attachments/assets/91d79cf6-fc24-42a7-8243-1c0356db480f)


En un principio no encontramos nada más.

## 2. COMPROMISO DEL ENTORNO

Probamos un ataque de fuerza bruta por ssh y el usuario russoski

```jsx
hydra -l russoski -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10
```

![image 10](https://github.com/user-attachments/assets/727cbf72-71da-499a-b64b-97a3cf37449c)


Encontramos la contraseña y accedemos por ssh

![image 11](https://github.com/user-attachments/assets/ac9c62ee-084a-4f53-9038-475f853bf6ce)


## 3. ELEVACION DE PRIVILEGIOS

A través del binario VIM podemos acceder a root sin falta de contraseña.

Buscamos en gtfobins la elevación de privilegios

![image 12](https://github.com/user-attachments/assets/a11d0ebc-6932-4337-88e9-c924ca25e03d)

![image 13](https://github.com/user-attachments/assets/e9e03d75-c78e-4f89-8261-852383c87cdb)

![image 14](https://github.com/user-attachments/assets/23bc924d-2718-485c-aa45-c91c6d01d378)


