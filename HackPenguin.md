# Writeup

## Levantar la máquina

![captura-levantar](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/2dd777cf-ea5c-420f-bcbd-556019531ee1)

## Conectividad con la máquina atacante

Vemos que hay conectividad con la máquina y sabemos que es Linux por el **TTL**, que es **64**. Aunque no siempre es fiable.

![captura-ping](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/ca814cf9-44a8-460c-b73e-074326a7118e)

## Escaneo de puertos

Usamos la herramienta **nmap** para el escaneo de puertos.

`sudo nmap -p- -sS -sC -sV --min-rate=5000 -n -vvv -Pn 172.17.0.2 -oN all`

Este comando lo que hace es buscar todos los puertos abiertos (1-65535) (`-p-`), lo hace de manera sigilosa (`-sS`), busca la versión de los servicios ( `-sV`), de manera rápida(`--min-rate=5000`), desactiva la resolución DNS para evitar problemas(`-n`), utiliza triple verbose para que tengamos más detalle del escaneo (`-vvv`)y omite la detección de host(`-Pn`). Lo guarda en el fichero all (`-oN all`).

![captura-nmap](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/a3598dce-466a-4e64-9c39-16d72cdeb28b)

Puertos **abiertos**

Puerto **80 -> HTTP**

Puerto **22 -> SSH**

## Fuzzing

Hacemos fuzzing con la herramienta **gobuster** y encontramos el siguiente archivo llamado **penguin.html**.

![captura-fuzzing](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/dcf8e153-b022-43d1-96af-64ea2a90249a)

Vemos lo siguiente al buscar en el navegador...

![captura-penguin](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/4297b721-3c61-4cc3-8dee-f21b343c025f)

Parece que **penguin** es un usuario, vamos a probar ataque de fuerza bruta.

## Hydra

