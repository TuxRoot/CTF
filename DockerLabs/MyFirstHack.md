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

## 2. COMPROMISO DEL ENTORNO

Buscamos vulnerabilidades de la versión y encontramos lo siguiente 

![image](https://github.com/user-attachments/assets/86ef7553-a9bc-44c5-ace7-38532cf74ff6)


## 3. ESCALADA DE PRIVILEGIOS

Hay una vulnerabilidad: CVE-2011-2523

Buscando exploit en internet encontramos lo siguiente https://www.exploit-db.com/exploits/49757 donde copiamos el script y ejecutamos, obtenido root en la máquina

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/04a726a8-94ce-4d9c-861c-5ef0f50a03ab/3149a61f-e2dd-4279-ac48-d2dca9edc963/image.png)

La escalación también se puede realizar a través de metasploit

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/04a726a8-94ce-4d9c-861c-5ef0f50a03ab/111ef160-529f-43bd-95b2-4264ec03e4e9/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/04a726a8-94ce-4d9c-861c-5ef0f50a03ab/5ed7179d-cb73-4f75-8ef7-2672036235b3/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/04a726a8-94ce-4d9c-861c-5ef0f50a03ab/18fd46c8-071d-4483-a9b8-94a2a1393327/image.png)
