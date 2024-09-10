# BREAKMYSSH

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

![image.png](BREAKMYSSH%209404ab199b864dc1832daaaa2e75c830/image.png)

## 2. COMPROMISO DEL ENTORNO

![image.png](BREAKMYSSH%209404ab199b864dc1832daaaa2e75c830/image%201.png)

Comprobamos que usa una versión antigua de OpenSSH

Abrimos metasploit para comprobar en la enumeracion de ssh que el usuario root es uno usuario valido

![image.png](BREAKMYSSH%209404ab199b864dc1832daaaa2e75c830/image%202.png)

![image.png](BREAKMYSSH%209404ab199b864dc1832daaaa2e75c830/image%203.png)

## 3. ESCALACION DE PRIVILEGIOS

Intentamos un ataque de fuerzo bruta con Hydra y el usuario root

Encontramos la contraseña del usuario root

![image.png](BREAKMYSSH%209404ab199b864dc1832daaaa2e75c830/image%204.png)

![image.png](BREAKMYSSH%209404ab199b864dc1832daaaa2e75c830/image%205.png)