# Writeup

![image](https://github.com/user-attachments/assets/1a24ac12-b972-42e6-b824-2f3b529b29d4)

## Levantar la máquina

![captura-arranque](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/5428d9e6-e656-4c3d-aa03-7fa63fbd8454)

## Conectividad con la máquina atacante

Vemos que hay conectividad con la máquina y sabemos que es Linux por el **TTL**, que es **64**.

![captura-ping](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/fc24939f-220f-4310-860c-0133105c905f)

## Escaneo de puertos

Usamos la herramienta **nmap** para el escaneo de puertos.

`sudo nmap -p- -sS -sC -sV --min-rate=5000 -n -vvv -Pn 172.17.0.2 -oN abiertos`

Este comando lo que hace es buscar todos los puertos abiertos (1-65535) (`-p-`, lo hace de manera sigilosa (`-sS`), busca la versión de los servicios ( `-sV`), de manera rápida(`--min-rate=5000`), desactiva la resolución DNS para evitar problemas(`-n`), utiliza triple verbose para que tengamos más detalle del escaneo (`-vvv`)y omite la detección de host(`-Pn`). Lo guarda en el fichero allPorts (`-oN allPorts`).

![captura-escaneo](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/6325e4c3-4c66-4f37-8a8f-13ae1cadf981)

Puertos **abiertos**

Puerto **80 -> HTTP**

## Fuzzing

Utilizamos la herramienta **gobuster** para encontrar directorios y archivos.

![captura-gobuster](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9d087f2a-f92c-42f4-9b97-57289f531834)

Vemos que al buscar el **upload.php** sale esto.

![captura-php](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/af42f71d-8c3f-4cd1-a9c4-c605eaf6b876)

## Reverse shell

Vemos que podemos subir archivos, por lo que podemos subir una reverse shell para ganar acceso.

![captura-upload](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/51f3fdfc-762e-4743-9a28-98cfde8c3285)

Ahora buscamos donde se ha subido la reverse shell. Si miramos en los directorios que ha encontrado **gobuster**, vemos que hay un directorio llamado **uploads**.

![captura-encontrar](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/88c2d777-226d-49b5-a74f-24e020bc776e)

En este caso voy a utilizar el puerto **443** en escucha. Y la IP, es la nuestra, la de Kali Linux.

![captura-reverse](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/d38ac2fa-549d-4170-b367-822a1916649e)

`nc -nlvp 443`

![captura-reverseshell](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/e87bf22d-53bf-4735-b98d-ec8d531b7ef6)

## Escalada de privilegios

Vemos que podemos ejecutar como **root** el comando **env**

![captura-sudo](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/e1edf513-43a7-43ac-a6cf-a06de6f272d0)

Entonces podemos hacer lo siguiente para conseguir una shell como root.

![captura-root](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/e202f8c0-df8a-48f8-9379-f388a29d4030)

Y ya somos root!!
