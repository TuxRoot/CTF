# INJECTION

## 0. HERRAMIENTAS

- SQL Injection
- Binario ENV

## 1. ENUMERACION

Realizamos un escaneo de puertos

```jsx
sudo nmap -p- --open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 172.17.0.2
```

Vemos los puertos abiertos:

- 22
- 80

![image](https://github.com/user-attachments/assets/85aa4e75-2746-4be7-931c-bdf7975a1b43)


## 2. COMPROMISO DEL ENTORNO

Accedemos a la url y comprobamos que es un panel de login

![image 1](https://github.com/user-attachments/assets/1e5e63ff-daad-4a37-973b-032689a22c0b)


Pobaremos una inyección SQL simple: **' or 1=1-- -**

![image 2](https://github.com/user-attachments/assets/48895e50-eb9e-4a5e-a88a-fc6e31420887)


Nos muestra lo siguiente:

![image 3](https://github.com/user-attachments/assets/0bf60867-c369-4882-8817-041e2c936307)


Aquí ya tenemos un usuario y contraseña

Probamos el acceso por ssh

![image 4](https://github.com/user-attachments/assets/ff0f4749-d3b0-4578-a525-a84208655121)


Probamos el sudo -l, pero vemos que no tiene

![image 5](https://github.com/user-attachments/assets/16eb21a6-fd7e-4b98-b87c-e6fed2ddc192)


## 3. ESCALACION DE PRIVILEGIOS

Probamos a buscar binarios 

```jsx
find / -perm -4000 2>/dev/null
```

![image 6](https://github.com/user-attachments/assets/2685530a-e66a-4d97-ab2f-31e33c3cf801)


Encontramos el binario /usr/bin/env

En gtfobins buscamos como escalar privilegios con este binario

![image 7](https://github.com/user-attachments/assets/7c0b4b23-8c2a-4827-b397-01a1b13f1981)

![image 8](https://github.com/user-attachments/assets/097b53f1-3c39-472d-9dc5-7df27d25654c)
