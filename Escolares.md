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




 n
