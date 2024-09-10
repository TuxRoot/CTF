# OBSESSION

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

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image.png)

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%201.png)

La url no parece tener nada, en cambio vemos en el código fuente la siguiente línea

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%202.png)

Hacemos un escaneo de directorios y archivos

```jsx
sudo gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '[http://172.17.0.2](http://172.17.0.2/)' -x 'html,txt,php,py
```

Vemos los siguientes directorios

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%203.png)

en /important tenemos el siguiente archivo

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%204.png)

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%205.png)

En backup encontramos lo siguiente, donde nos confirma un usuario “russoski”

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%206.png)

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%207.png)

Para ver si encontramos algo más. Probaremos acceso a través del ftp, puerto 21, ya que se puede acceder con sesión anónima.

Vemos que tenemos 2 archivos

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%208.png)

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%209.png)

En un principio no encontramos nada más.

## 2. COMPROMISO DEL ENTORNO

Probamos un ataque de fuerza bruta por ssh y el usuario russoski

```jsx
hydra -l russoski -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 10
```

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%2010.png)

Encontramos la contraseña y accedemos por ssh

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%2011.png)

## 3. ELEVACION DE PRIVILEGIOS

A través del binario VIM podemos acceder a root sin falta de contraseña.

Buscamos en gtfobins la elevación de privilegios

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%2012.png)

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%2013.png)

![image.png](OBSESSION%20e5c71366f21445e8ae1e21791ff34028/image%2014.png)