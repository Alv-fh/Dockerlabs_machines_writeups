# Writeup

## Levantar la máquina

![captura-levantar](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/538dd08e-411a-4379-8ad0-2c2ac5774247)

## Conectividad con la máquina atacante

Vemos que hay conectividad con la máquina y sabemos que es Linux por el **TTL**, que es **64**. Aunque no siempre es fiable.

![captura-ping](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f83d4bf7-cf04-416f-bcf8-8c6b65d7cad9)

## Escaneo de puertos

Usamos la herramienta **nmap** para el escaneo de puertos.

`sudo nmap -p- -sS -sC -sV --min-rate=5000 -n -vvv -Pn 172.17.0.2 -oN all`

Este comando lo que hace es buscar todos los puertos abiertos (1-65535) (`-p-`), lo hace de manera sigilosa (`-sS`), busca la versión de los servicios ( `-sV`), de manera rápida(`--min-rate=5000`), desactiva la resolución DNS para evitar problemas(`-n`), utiliza triple verbose para que tengamos más detalle del escaneo (`-vvv`)y omite la detección de host(`-Pn`). Lo guarda en el fichero all (`-oN all`).

![captura-escaneo](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b1f3a44d-2a98-41a0-b1e8-f1206e26995d)

Puertos **abiertos**

Puerto **80 -> HTTP**

## Fichero /etc/hosts

Como podemos ver, la página no resuelve ya que comprobamos con el reporte de **nmap** que tiene un dominio llamado **hidden.lab**.
Por los tanto lo añadimos al fichero /etc/hosts.

![captura-host](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/3cb44c48-71bd-4ca2-ae82-49127d023bf3)

Ahora si vemos que resuelve bien la página.

![captura-cafe](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/979900f2-c8cf-475b-8611-a7e378db6f98)

## Fuzzing

Para empezar hago fuzzing para conocer directorios que cuelguen de la raiz pero veo que no hay ninguno relevante por lo que se me ocurre a hacer fuzzing de subdominios con la herramienta **wfuzz** y encuentro el siguiente.

`wfuzz -c --hl=9 -w /usr/share/wordlist/SecLists/Discovery/DNS/subdomain-top1million-110000.txt -H "Host:FUZZ.hidden.lab" -u 172.17.0.3`

![captura-subdominio](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/1aed61f6-3fe2-428d-97b9-e526fa6f3478)

Y lo volvemos a añadir al fichero **/etc/hosts** para que pueda resolver bien.

![captura-sub](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/8bb202aa-e2f4-4cf0-bb0f-bbec127a6950)

Vemos que podemos subir archivos, pero solo con extensión PDF.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/1706dce2-19dd-4e65-8d48-390d93017ce9)

Busco por Internet extensiones que permitan **php** y tras probar extensiones encuentro en HackTricks la siguiente extensión.

**[.phar](https://book.hacktricks.xyz/pentesting-web/file-upload)**.

Por lo que puedo usar la herramienta **[php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell)** para conseguir una reverse shell




