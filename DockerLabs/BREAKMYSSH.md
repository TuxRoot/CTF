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

![image](https://github.com/user-attachments/assets/402e416a-3794-4d9b-884f-7dd59415ea54)


## 2. COMPROMISO DEL ENTORNO

![image 1](https://github.com/user-attachments/assets/f0aaa033-d40e-4612-9d5a-83948076ae6a)


Comprobamos que usa una versión antigua de OpenSSH

Abrimos metasploit para comprobar en la enumeracion de ssh que el usuario root es uno usuario valido

![image 2](https://github.com/user-attachments/assets/967e557c-5646-4456-864e-581da4325333)

![image 3](https://github.com/user-attachments/assets/2c30c275-a953-4ce2-84ea-13c4bfb8f3da)


## 3. ESCALACION DE PRIVILEGIOS

Intentamos un ataque de fuerzo bruta con Hydra y el usuario root

Encontramos la contraseña del usuario root

![image 4](https://github.com/user-attachments/assets/063a253b-4dfc-47b5-a363-0d6e65b5b82e)

![image 5](https://github.com/user-attachments/assets/0e85542b-f740-4107-b5d2-34fb83725348)
