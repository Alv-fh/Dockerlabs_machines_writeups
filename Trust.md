# Writeup

![image](https://github.com/user-attachments/assets/b93aa5be-e943-464e-80e6-e4d5d7721685)


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

Puertos **abiertos**
Puerto **80 -> HTTP**
Puerto **22 -> SSH**

## Fuzzing

Hacemos fuzzing al puerto 80 para ver si cuelga algun directorio o archivo de la raiz.

![captura-fuzzing](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9cc6c4a6-126f-45bb-b634-5b91ee77efd8)

Si buscamos el archivo encontrado, sale esto.

![captura-80](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/8b54546a-74cb-4df7-a7fe-a18196c752ba)

Parece que **mario** es un usuario.
Tenemos otra posibilidad de ataque, que es por SSH.

## Hydra

Utilizamos **hydra** para hacer un ataque de diccionario por SSH.

![captura-22](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/2fdeb621-5987-491f-a041-4a8c3332666e)

Entramos y vemos que podemos ejecutar el comando **vim**, que es un editor de texto.

![captura-vim](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f3fcd5b9-9a9f-443c-a934-dd4b91ba8965)

Sabemos que para poder empezar a escribir en vim, necesitamos poner `:`
Vemos que al abrir vim podemos crear una shell si ponemos `!/bin/bash`, acompañado de los dos puntos.

![captura-vim2](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/dc53e16f-e447-4dc9-b235-87cb197472c3)

Y ya somos root!!

![captura-root](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/3bb918a8-4426-4e86-86f5-04f2fc99ad6c)

