# Writeup

## Levantar la máquina

![captura-arranque](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/5428d9e6-e656-4c3d-aa03-7fa63fbd8454)

## Conectividad con la máquina atacante

Vemos que hay conectividad con la máquina y sabemos que es Linux por el **TTL**, que es **64**.

![captura-ping](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/fc24939f-220f-4310-860c-0133105c905f)

## Escaneo de puertos

Usamos la herramienta **nmap** para el escaneo de puertos.

`sudo nmap -p- -sS -sS -sV --min-rate=5000 -n -vvv -Pn 172.17.0.2 -oN abiertos`

Este comando lo que hace es buscar todos los puertos abiertos (1-65535) (`-p-`, lo hace de manera sigilosa (`-sS`), busca la versión de los servicios ( `-sV`), de manera rápida(`--min-rate=5000`), desactiva la resolución DNS para evitar problemas(`-n`), utiliza triple verbose para que tengamos más detalle del escaneo (`-vvv`)y omite la detección de host(`-Pn`). Lo guarda en el fichero allPorts (`-oN allPorts`).

![captura-escaneo](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/6325e4c3-4c66-4f37-8a8f-13ae1cadf981)

Puertos **abiertos**
Puerto **80 -> HTTP**

## Fuzzing

Utilizamos la herramienta **gobuster** para encontrar directorios y archivos.

![captura-gobuster](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9d087f2a-f92c-42f4-9b97-57289f531834)

Vemos que al buscar el **upload.php** sale esto.

![captura-php](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/af42f71d-8c3f-4cd1-a9c4-c605eaf6b876)
