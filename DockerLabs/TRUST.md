# TRUST

## 0. HERRAMIENTAS

- Hydra
- Binario VIM

## 1. ENUMERACION

Realizamos un escaneo de puertos

```jsx
sudo nmap -p- --open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 172.17.0.2
```

Vemos los puertos 22 y 80 abiertos

Cargamos la url y vemos la página por defecto de Apache

![image](https://github.com/user-attachments/assets/98e69a04-0cd9-4fbf-ab0d-09642178ff55)


En el código fuente tampoco vemos nada relevante.

Lanzamos un escaneo de url para buscar subdirectorios y ficheros

```jsx
 sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '[http://172.17.0.2](http://172.17.0.2/)' -x 'html,txt,php,py'
```

Descubrimos el archivo secret.php

![image 2](https://github.com/user-attachments/assets/f42dd13c-081b-4c5c-836f-8685dfaa56f1)
![image 1](https://github.com/user-attachments/assets/63ed7238-3b0e-4501-b617-a7c9b8f22cd2)


En esa url podemos identificar un nombre a tener en cuenta

- mario

## 2. COMPROMISO DEL ENTORNO

Probamos a lanzar un ataque de fuerza bruta al usuario mario, protocolo ssh

```jsx
hydra -l mario -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10
```

Encontramos la contraseña y procedemos a acceder con el usuario mario


![image 3](https://github.com/user-attachments/assets/ed0a3e98-0549-4fef-99a3-e4321a34c434)
![image 4](https://github.com/user-attachments/assets/c74731d9-4a72-432c-b234-a6a52cfb8a29)

## 3. ELEVACION DE PRIVILEGIOS

Comprobamos que podemos acceder a traves de un sudo -l 

![image 5](https://github.com/user-attachments/assets/492aed16-0281-465a-8777-01267687ce12)


y que podemos ejecutar el binario vim con cualquier usuario

Accedemos a gtfobins y comprobamos que se puede elevar privilegios a root con este binario

![image 6](https://github.com/user-attachments/assets/75232c5d-5ab5-49b8-8824-bdd27a579135)
![image 7](https://github.com/user-attachments/assets/5033c45b-0eb5-400f-bcae-5cea34e38c58)

