## 0. HERRAMIENTAS

- Vulnerabilidad CVE-2011-2523
- Script
- Metasploit

## 1. ENUMERACIÓN

Hacemos un escaneo de puertos abiertos con nmap

```jsx
sudo nmap -p- --open -sS -sC -sV --min-rate=5000 -vvv -n -Pn -oN escaneo 172.17.0.2
```

Vemos que está abierto el puerto 21, protocolo FTP, con la versión vsftpd 2.3.4

![image](https://github.com/user-attachments/assets/3405fedd-0146-4b2c-a1ea-acb034bda4d5)


## 2. COMPROMISO DEL ENTORNO

Buscamos vulnerabilidades de la versión y encontramos lo siguiente 

![image](https://github.com/user-attachments/assets/86ef7553-a9bc-44c5-ace7-38532cf74ff6)


## 3. ESCALADA DE PRIVILEGIOS

Hay una vulnerabilidad: CVE-2011-2523

Buscando exploit en internet encontramos lo siguiente https://www.exploit-db.com/exploits/49757 donde copiamos el script y ejecutamos, obtenido root en la máquina

![image](https://github.com/user-attachments/assets/941283bc-86dc-4dfd-a5a6-2d6c308491e3)


La escalación también se puede realizar a través de metasploit

![image](https://github.com/user-attachments/assets/20ad1843-f146-4d89-b68e-42172d5c0404)


![image](https://github.com/user-attachments/assets/6646b04b-7026-4b3c-8b20-ee37e8ecff10)


![image](https://github.com/user-attachments/assets/1cb3a9a8-c5b7-44f3-aebf-9541c597c45b)

