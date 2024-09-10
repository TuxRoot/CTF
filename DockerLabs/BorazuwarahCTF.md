# BorazuwarahCTF

## 0. HERRAMIENTAS

- Hydra
- metasploit

## 1. ENUMERACION

Realizamos un escaneo de puertos

```jsx
sudo nmap -p- --open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 172.17.0.2
```

Vemos los puertos abiertos:

- 22
- 80

![image](https://github.com/user-attachments/assets/2abab65b-b4b0-4f60-bbe5-ce340dec6e86)


La url muestra la siguiente imagen

![image 1](https://github.com/user-attachments/assets/cf78fac0-cb74-4732-8fea-7c7f7fa353c6)


Guardamos la imagen y pasamos a realizar una búsqueda de directorios

```jsx
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '[http://172.17.0.2](http://172.17.0.2/)' -x 'html,txt,php,py,jpeg'
```

![image 2](https://github.com/user-attachments/assets/75e781ff-1941-4e6e-9f7e-95dcd734dfea)


Solo aparece la imagen que hemos encontrado antes

![image 3](https://github.com/user-attachments/assets/8e9180e0-df00-4f07-9686-b1e4b957e191)


Analizando metadatos de la imagen vemos que hay oculto un usuario

![image 4](https://github.com/user-attachments/assets/78c36bbc-8595-4992-80b2-8bd27ebe1c8d)


## 2. COMPROMISO DEL ENTORNO

Utilizaremos Hydra para intentar de obtener la contraseña por fuerza bruta

```jsx
hydra -l borazuwarah -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10
```

Encontramos la contraseña

![image 5](https://github.com/user-attachments/assets/e35c92d7-27cd-4160-bdb9-6f7db5730ffd)


Accedemos por ssh

![image 6](https://github.com/user-attachments/assets/c0c1e75b-96ee-44be-8b3b-47aee811b6a9)


## 3. ESCALACION DE PRIVILEGIOS

Vemos que se puede ejecutar desde cualquier usuario y sin contraseña el /bin/bash

![image 7](https://github.com/user-attachments/assets/4a481325-ac23-4ec8-ba69-cdd5fafdd539)

![image 8](https://github.com/user-attachments/assets/caab4c38-8bc3-476d-85d5-dab9c97a0e31)
