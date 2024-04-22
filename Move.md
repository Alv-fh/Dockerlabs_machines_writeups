# Writeup

## Levantar la máquina

![captura-arranque](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b6d2ac9d-c6f8-41f4-9aaf-5b3106442156)

## Conectividad con la máquina atacante

Vemos que hay conectividad con la máquina y sabemos que es Linux por el **TTL**, que es **64**.

![captura-ping](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/09077e25-7734-43ff-9ab0-5bc4c2b6504d)

## Escaneo de puertos

Usamos la herramienta **nmap** para el escaneo de puertos.

`sudo nmap -p- -sS -sC -sV --min-rate=5000 -n -vvv -Pn 172.17.0.2 -oN puertos`

Este comando lo que hace es buscar todos los puertos abiertos (1-65535) (`-p-`, lo hace de manera sigilosa (`-sS`), busca la versión de los servicios ( `-sV`), de manera rápida(`--min-rate=5000`), desactiva la resolución DNS para evitar problemas(`-n`), utiliza triple verbose para que tengamos más detalle del escaneo (`-vvv`)y omite la detección de host(`-Pn`). Lo guarda en el fichero allPorts (`-oN puertos`).

![captura-puertos](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/a4043998-5bce-4d9c-8ff7-a77a2a6eee03)

Puertos **abiertos**

Puerto **21 -> FTP**

Puerto **22 -> SSH**

Puerto **80 -> HTTP**

Puerto **3000 -> Grafana**

## Fuzzing

Si hacemos fuzzing con **gobuster** vemos que encuentra un archivo llamado **maintenance.html**.

![captura-fuzzing](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b9b0f6b9-3e0e-4e55-b197-690e36ffda2d)

Lo buscamos en el navegador.

![captura-tmp](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/cbe4f6f9-655c-47c6-9710-a1688f304016)

## Grafana

### Reconocimiento del puerto 3000

Si buscamos en el navegador la IP de la máquina por el puerto 3000, vemos que está corriendo un [Grafana](https://grafana.com/).

![captura-grafana](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/8428a472-ae16-46eb-8555-7f089d0a2d9e)

Buscamos un exloit para la version **8.3.0** ya que vemos en la parta inferior que utiliza esa versión.

![captura-exploit](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9bb4cf6f-b243-4a69-8025-26232e82bb6b)

Vemos que lo único que tenemos que poner es la flag **-H** y la IP. También indicaremos el puerto **3000** que es por donde está corriendo [Grafana](https://grafana.com/). Y este exploit lo que hace es leer archivos del sistema.

![captura-passwd](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f72c99b2-52f6-463b-9b11-08b11de82f46)

Vemos que existe un usuario llamado **freddy**.

Podriamos intentar con la herramienta **hydra** hacer un ataque de fuerza bruta pero por ahí no van los tiros.

Si recordamos, tenemos el puerto **21** abierto, y en el reporte de **nmap** vemos que podemos entrar como **Anónimo**.

![captura-ftp](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/0e59c426-056f-4ebd-83ba-1d45138ab864).

Si buscamos en internet vemos que el archivo **database.kdbx** se puede desencriptar con [Keepassxc](https://keepassxc.org/).

Si recordamos, nos salía haciendo **fuzzing** un archivo llamado **maintenance.html**, en el que dice que el sitio web está en mantenimiento, que acceda a **/tmp/pass.txt**.

Si volvemos a utilizar el exploit de python...

![captura-pass txt](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/bed8d985-3cf6-49b8-b4f6-d535c8062787)

Entonces tiene pinta de que es la clave para poder acceder a la base de datos. Utilizamos la aplicación.

![captura-keepass](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/98b19e03-280f-4db2-b55b-e449d5491866)

Y vemos la clave de freddy. Por lo que podemos probar por SSH ya que si recordamos, el puerto estaba abierto.

![captura-freddy](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/24d05b02-627b-43f2-b47d-ab78c3fd56df)

Entramos con freddy y vemos que está corriendo un Kali Linux

Lo siguiente es entrar con root.

![captura-freddy2](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/8f8f56f9-c647-4688-a8e4-e85d74d57c6b)

Vemos que podemos ejecutar como cualquier usuario el script de python llamado maintenance.py

![captura-sudo](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/337dd9c0-ce51-4e6a-82cd-6ce6d0d03fe3)

Entramos en él para ver lo que hay.
