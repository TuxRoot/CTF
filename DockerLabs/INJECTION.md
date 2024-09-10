# INJECTION

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

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image.png)

## 2. COMPROMISO DEL ENTORNO

Accedemos a la url y comprobamos que es un panel de login

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image%201.png)

Pobaremos una inyección SQL simple: **' or 1=1-- -**

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image%202.png)

Nos muestra lo siguiente:

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image%203.png)

Aquí ya tenemos un usuario y contraseña

Probamos el acceso por ssh

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image%204.png)

Probamos el sudo -l, pero vemos que no tiene

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image%205.png)

## 3. ESCALACION DE PRIVILEGIOS

Probamos a buscar binarios 

```jsx
find / -perm -4000 2>/dev/null
```

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image%206.png)

Encontramos el binario /usr/bin/env

En gtfobins buscamos como escalar privilegios con este binario

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image%207.png)

![image.png](INJECTION%20935405421d254d14b62c06725839a533/image%208.png)