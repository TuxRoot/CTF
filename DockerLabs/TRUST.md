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

![image.png](TRUST%20143bc5e353034a45acf40a11e044ac16/image.png)

En el código fuente tampoco vemos nada relevante.

Lanzamos un escaneo de url para buscar subdirectorios y ficheros

```jsx
 sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '[http://172.17.0.2](http://172.17.0.2/)' -x 'html,txt,php,py'
```

Descubrimos el archivo secret.php

![image.png](TRUST%20143bc5e353034a45acf40a11e044ac16/image%201.png)

![image.png](TRUST%20143bc5e353034a45acf40a11e044ac16/image%202.png)

En esa url podemos identificar un nombre a tener en cuenta

- mario

## 2. COMPROMISO DEL ENTORNO

Probamos a lanzar un ataque de fuerza bruta al usuario mario, protocolo ssh

```jsx
hydra -l mario -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10
```

Encontramos la contraseña y procedemos a acceder con el usuario mario

![image.png](TRUST%20143bc5e353034a45acf40a11e044ac16/image%203.png)

![image.png](TRUST%20143bc5e353034a45acf40a11e044ac16/image%204.png)

## 3. ELEVACION DE PRIVILEGIOS

Comprobamos que podemos acceder a traves de un sudo -l 

![image.png](TRUST%20143bc5e353034a45acf40a11e044ac16/image%205.png)

y que podemos ejecutar el binario vim con cualquier usuario

Accedemos a gtfobins y comprobamos que se puede elevar privilegios a root con este binario

![image.png](TRUST%20143bc5e353034a45acf40a11e044ac16/image%206.png)

![image.png](TRUST%20143bc5e353034a45acf40a11e044ac16/image%207.png)