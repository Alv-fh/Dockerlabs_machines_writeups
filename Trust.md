# Writeup

## Levantar la máquina

![captura-levantar](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9d4239a7-fab0-428c-ba60-49dc1928efa3)

## Conectividad con la máquina atacante

Comprobamos que tenemos conectividad y vemos que tiene **ttl 64**, por lo que se trata de máquina Linux. Aunque no siempre es así.

![captura-ping](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9c210269-1707-409b-b54c-d13bfbee5415)

## Escaneo de puertos

Utilizamos la herramienta **nmap** para escanear los puertos abiertos.

`sudo nmap -p- -sS -sC -sV --min-rate=5000 -n -vvv -Pn 172.17.0.2 -oN puertos`

Este comando lo que hace es buscar todos los puertos abiertos (1-65535) (`-p-`, lo hace de manera sigilosa (`-sS`), busca la versión de los servicios ( `-sV`), de manera rápida(`--min-rate=5000`), desactiva la resolución DNS para evitar problemas(`-n`), utiliza triple verbose para que tengamos más detalle del escaneo (`-vvv`)y omite la detección de host(`-Pn`). Lo guarda en el fichero allPorts (`-oN allPorts`).

![captura-puertos](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/6059c2fc-aac3-45c8-8898-3b6913ee5de7)

Puertos **80 -> HTTP** y **22 -> SSH** abiertos.

## Fuzzing

Hacemos fuzzing al puerto 80 para ver si cuelga algun directorio o archivo de la raiz.

![captura-fuzzing](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9cc6c4a6-126f-45bb-b634-5b91ee77efd8)
