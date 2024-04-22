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

Busco por Internet extensiones que permitan **php** y tras probar extensiones encuentro en HackTricks la extensión **[.phar](https://book.hacktricks.xyz/pentesting-web/file-upload)**.

Por lo que puedo usar la herramienta **[php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell)** para conseguir una reverse shell.

Editamos el archivo y ponemos en la IP, la nuestra, la de Kali Linux, es decir, la atacante, y el puerto que vamos a utilizar, en mi caso, el **443**.

![captura-php](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f88361b3-3166-443b-b44e-94700530f8e6)

Y ahora le cambiamos la extensión e intentamos subirlo. Para cambiar la extensión podemos hacer uso del **mv**

`mv php-reverse-shell.php  php-reverse-shell.phar`

¡¡ IMPORTANTE !!

Es importante que en la esquina inferior derecha pongais todos los archivos, porque sino, no os saldrá.

![captura-todos](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b8d7034f-f726-432a-a8d7-87802de08f34)

Una vez subido, tenemos que saber donde se ha subido, para ello podemos volver a hacer fuzzing con el nuevo subdominio para saber donde se ha subido.

![captura-uploads](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/15c1573c-f9ca-4da2-ab3e-e40915e73f3b)

Entramos y vemos que si está ahí subido, entonces ahora hacemos uso del **nc** para obtener la reverse shell.

![captura-nc](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/6e2811e0-efbe-4958-9ee3-893a5a3a1178)

Y si se queda cargando significa que la hemos obtenido.

![captura-reverse](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/8b69b9ae-130f-42af-907b-590ee849b5c2)

`script /dev/null -c bash`

`CTRL + Z`

`stty raw -echo; fg`

`reset xterm`

`export SHELL=bash`

`export TERM=xterm`

`stty rows 41 columns 189`

![captura-bonito](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/df86f70c-038f-44f5-bcef-b928e9f1ac76)

Tras intentar buscar binarios con permisos SUID, hacer uso del `sudo -l` etc, no encuentro nada y decido hacer ataque de fuerza bruta con usuarios del sistema.

![captura-passwd](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/adf52a68-0a4d-4f54-8265-680161f2177e)

Vemos que existen **cafetero, john, bobby**.

Subimos un script en bash que hace ataque de fuerza bruta con diccionario, para ello, he acortado el rockyou.txt a 200 palabras ya que no me dejaba subirlo más grande.

`head -n 200 rockyou.txt > rockyou_200.txt`

Subimos el diccionario.

El script que voy a utilizar es este -> [Linux-Su-Force](https://github.com/Maalfer/Sudo_BruteForce/blob/main/Linux-Su-Force.sh)

Subimos el script.

![captura-todo](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/77c89cff-653e-48c0-a52d-e960ed171bc6)

Ahora volvemos a la reverse shell y ejecutamos el script.

Le damos permisos y ejecutamos.

`chmod +x Linux-Su-Force.sh`

`bash Linux-Su-Force.sh cafetero rockyou_200.txt`

![captura-cafetero](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/a2757b05-2043-48fe-b7bc-ca44d01336c1)

Ahora entramos como cafetero y vemos que podemos usar con **sudo**.

![captura-nano](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f3b625ce-404f-45b9-83fe-f98ebd92e2a0)

Hago uso de la herramienta [Searchbins](https://github.com/r1vs3c/searchbins) que está basada en la página [GTFOBins](https://gtfobins.github.io/).

![captura-searchbins](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/1f84a783-3698-4452-aaf4-01c6953c8fa1)

Y lo ponemos en práctica.

![captura-john](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/152e84ab-2376-4dc4-8307-312b6362bacb)

`sudo -u john /usr/bin/nano`

`^R -> (Ctrl R) ^X -> (Ctrl X`

`reset; sh 1>&0 2>&0`

Vemos que funciona, así que ahora vamos a por el siguiente usuario.

![captura-bobby](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/6f5efc27-2600-42bf-bb92-d3ddbeb9b24e)

Hacemos uso de la herramienta.

![captura-apt](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/3f6400df-a17c-4c28-9ed8-bfa4a182ede3)

`sudo -u bobby /usr/bin/apt changelog apt`

`!/bin/sh`

Probamos el primero...

![captura-bobby2](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/6aaecb49-e7ac-4f82-897c-b720dd9906f8)

Ganamos acceso a bobby. Vamos a por el siguiente que ya es root.

![captura-roo](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/c1f09b1d-8bb1-49ba-8f7d-0cacde6c71d4)

Vemos que podemos ejecutar como root el binario find, por lo que buscamos en nuestra herramienta.

`sudo -u root /usr/bin/find . -exec /bin/sh -p \; -quit`

![captura-find](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/2f784669-2d72-4ce7-a668-73f1b2f806ba)

Y vemos que funciona correctamente.

![captura-root](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9ec161a1-dbd6-48f9-a25a-6d3376838d7e)

Ya somos root!!
