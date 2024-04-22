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

Si hacemos fuzzing con **gobuster** vemos que encuentra un archivo llamado **maintenance.html**

![captura-fuzzing](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b9b0f6b9-3e0e-4e55-b197-690e36ffda2d)

Lo buscamos en el navegador.

![captura-tmp](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/cbe4f6f9-655c-47c6-9710-a1688f304016)

## Grafana

### Reconocimiento del puerto 3000

Si buscamos en el navegador la IP de la máquina por el puerto 3000, vemos que está corriendo un [Grafana](https://grafana.com/).

![captura-grafana](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/8428a472-ae16-46eb-8555-7f089d0a2d9e)


