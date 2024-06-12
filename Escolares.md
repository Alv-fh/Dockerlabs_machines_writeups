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

Vemos que en el reporte de **gobuster** encuentra una ruta llamada **phpmyadmin**. Entrramos y vemos esto:

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/eef5c66d-9b0c-46a6-b51c-fb398c95b65d)

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

Veo que luis es el admin, debajo en el correo pone luisillo por lo que puede que sea el usuario. Intento con **wpscan**

Tendremos que crear con la herramienta **cupp** un diccionario, y pondremos los datos que salen. Una vez generado, lo ponemos en el **wpscan**

`wpscan --url http://escolares.dl/wordpress -U luisillo --passwords luis.txt`

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f4eb66ed-6bf2-497f-bac7-7df7b993f601)

Encontramos la clave de luisillo

