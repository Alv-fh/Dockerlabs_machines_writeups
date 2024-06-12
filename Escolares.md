## Escolares

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/4c2d1970-d7ff-44dd-a833-e98e3843bd26)

## Conectividad con la máquina objetivo

Comprobamos que sí tenemos conectividad con la máquina y que el **TTL** es 64 por lo que hablamos de una máquina Linux.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9a8a2e4d-5fef-4281-ba2e-47569b47bad5)

## Escaneo de Puertos

Utilizamos la herramienta **nmap** y vemos los puertos abiertos.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/e5be85fa-ba08-4e29-bb74-67363ccd0184)

22 -> SSH

80 -> HTTP

## Fuzzing Web

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/d1586d08-6d99-4a65-b7cd-4e29e7264245)

También vemos que si le hacemos fuzzing a la ruta `http://ip/wordpress/` nos enumera esto:

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/cd9e357a-d30d-4540-a990-8edaf5ed919d)

Entramos en **trackback.php** y sale esto:

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/77af5199-cc6f-4537-acfb-d694e08b14ad)

En el fuzzing web descubro que hay un dominio ya que veo la ruta absoluta y veo que **http://escolares.dl/**, por lo que lo añado al fichero **/etc/hosts**

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/e4e29956-e55d-48d8-8320-e50e44be976e)

Si busco **xmlrpc.php** sale lo siguiente:

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/ecc381e5-9b32-48c0-a037-ac5d6b5ac10b)

Inspecciono la página y veo que hay un comentario en donde hace referencia a una ruta, por lo que decido ir.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/58b1ac8d-73cc-4e73-8df8-e7b724b54965)

Veo que luis es el admin, debajo en el correo pone luisillo por lo que puede que sea el usuario. Intento con **wpscan**.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f65dd3fb-8f8c-4d37-9b90-a6311474e9f4)

Tendremos que crear con la herramienta **[cupp](https://github.com/Mebus/cupp)** un diccionario, y pondremos los datos que salen. Una vez generado, lo ponemos en el **wpscan**.

`wpscan --url http://escolares.dl/wordpress -U luisillo --passwords luis.txt`.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f4eb66ed-6bf2-497f-bac7-7df7b993f601)

Encontramos la clave de luisillo. Y entramos

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/29ed76e6-ae4e-434c-bb38-3ba5fe60bb42)

Veo que hay un plugin llamado File Manager, por lo que podemos investigar ahí, y vemos que podemos subir archivos.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f00c7877-abab-4026-a2dc-3a2b7dd26c17)

Subimos una reverse shell en php -> [REVERSE-SHELL](https://github.com/pentestmonkey/php-reverse-shell)

Antes de nada, la editamos y ponemos nuestra ip de atacante y el puerto que vayamos a utilizar.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/681a0b68-28d2-424d-a47e-253cd5c53d19)

Nos ponemos en escucha.

`nc -nlvp 443`

Y buscamos la reverse shell en el navegador. Si se queda cargando, es buena señal.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/2f85231b-7b64-4223-bd19-4577351aa836)

Y conseguimos una shell. Hacemos el tratamiento de la **TTY** como aparece en pantalla.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/ed6b064e-bfe5-4054-afe2-826425fc2b53)

Vemos el **/etc/passwd**.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/48e55f9d-1502-4eca-88e8-c009adb17fdd)

Nos metemos en el home y encontramos un fichero llamado secret.txt en el parece ser la clave de luisillo.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/7f88d984-51a5-4087-a324-8921c7f2c02d)

Entramos como luisillo y vemos que podemos ejecutar como cualquier usuario el binario **awk**.

Buscamos en la herramienta [searchbins](https://github.com/r1vs3c/searchbins).

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/1a55a6e5-6a75-43d9-92fa-e7f518f95f40)

Lo ejecutamos en la máquina pero de esta manera para que no haya ningún error.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/bcb01c81-a609-4cce-a10b-3248247fd616)

Ya somos root!!
