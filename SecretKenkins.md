Secretjenkins

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/e8cb70a6-23fd-4fd0-ac1b-fb9fccc212fc)

## Conectividad con la máquina objetivo

Comprobamos que sí tenemos conectividad con la máquina y que el **TTL** es 64 por lo que hablamos de una máquina Linux.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/20e04743-84c9-4094-b613-03b701e5813e)

## Escaneo de Puertos

Utilizamos la herramienta **nmap** y vemos los puertos abiertos.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/a2867f67-8822-45cb-8490-b2d2e9acc2ce)

22 -> SSH

8080 -> Jenkins

Con la herramienta [nuclei](https://github.com/projectdiscovery/nuclei) sacamos la versión -> 2.441

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9c943394-8cfb-40db-ba2c-b67bf5346422)

Buscamos en internet y vemos que en Eploitdb hay un exploit de LFI en Python. Nos lo descargamos y vemos como funciona.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/3738fc12-f1cd-4f26-84f6-5490ad6b774b)

Con el `-h` vemos como funciona.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/d0fce717-9a00-44fa-a4dc-71057d395b69)

Vemos que se podemos ver el **/etc/passwd** y conocemos nuevos usuarios.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/dcd8522a-42bb-47a5-b751-d2b0b66610f1)

Recordamos que tenemos el puerto 22 abierto, por lo que podemos hacer un ataque de diccionario con **hydra**.

Pruebo con el usuario **pinguinito** pero no consigo sacarla, así que pruebo con el usuario **bobby** y la encuentro.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/21398369-71cb-4e39-b059-4748bca0e837)

Entro por SSH y veo que puedo ejecutar el binario **python3** con **pinguinito**

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/479ef880-88ff-4d4a-9a4a-8395cf7a3993)

Busco en la herramienta [searchbins](https://github.com/r1vs3c/searchbins). y encontramos esto.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/53a93eac-b047-4863-8566-a00981b6f9f8)

Entramos como pinguinito pero como sale en pantalla para evitar problemas y vemos que podemos ejecutar un script como cualquier usuario.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/bb14c286-f2ac-460a-b7ec-8fe23259055d)

Intentamos escribir en el script pero nos deniega el permiso.

Vemos que el propietario del script es **pinguinito** por lo que podemos intentar darle permisos de escritura.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/2b502a20-fa5d-48d2-ad47-7b65242daf2f)

Vemos que si nos deja con el comando `chmod +w /opt/script.py`

Entonces ahora probamos a añadir esta línea. Que es lo que habíamos hecho antes, pero ahora lo añadimos al script. Esto básicamente lo que hace es importar una librería llamada os, que puede ejecutar comandos del sistema operativo, y ejecuta una bash, entonces al ejecutarla como root, pues la bash es de root.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/72201948-c7bd-4b3d-9c26-da5ef38788e7)

Entonces la ejecutamos y...

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/a6eebec2-14a3-4325-b908-f0a1e89c45c0)

Ya somos root!!
