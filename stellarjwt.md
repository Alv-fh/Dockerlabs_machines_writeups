# stellarjwt

Desplegamos la máquina y vemos la IP.

![image](https://github.com/user-attachments/assets/f77eddb2-7029-49c8-a047-2dd1511bc57d)

## Fase de Reconocimiento

Hacemos un ping y mediante el TTL vemos que se trata de una máquina Linux ya que es 64.

![image](https://github.com/user-attachments/assets/7b3a80b0-fc7c-4f87-a957-70c041ee244d)

Hacemos un escaneo de nmap de tipo SYN para que sea rápido y sigiloso y vemos que tiene los puertos 22,80 abiertos.

![image](https://github.com/user-attachments/assets/7467a3bf-f94b-49e8-af86-3188edde0b81)

## Fase de Explotación

Entramos en la Web y la siguiente Página Web. En ella vemos esta pregunta:

![image](https://github.com/user-attachments/assets/c56f3581-c32b-4bdd-87de-fb26b85ceccd)

Buscamos en Internet y nos quedamos con el nombre Gottfried:

![image](https://github.com/user-attachments/assets/abc16f56-7b8f-4e9b-b173-83ade0e47435)

A continuación se me ocurre hacer fuzzing ya que no encuentro nada más.

/universe

![image](https://github.com/user-attachments/assets/de865021-2951-424c-a1c1-e9f2ba076c14)

Encontramos la siguiente ruta, por lo que entramos y vemos esta imagen.

![image](https://github.com/user-attachments/assets/cee96555-68b0-40a5-a44f-75c8b749e506)

A simple vista no vemos nada, podemos intentar hacer esteganografía, pero no va por ahí, entonces miramos el código fuente y si bajamos al final encontramos esta linea de código que a simple vista parece un JSON.

![image](https://github.com/user-attachments/assets/5fa91380-8bae-4bc6-b3a9-936b1a052ecc)

Buscamos en la Web [JWT](https://jwt.io/) y encontramos el primer usuario **neptuno**.

![image](https://github.com/user-attachments/assets/2515b0cd-0e0b-4e97-ad0f-d9e5ab4afbad)

Intentamos hacer fuerza bruta con hydra para el servicio SSH pero vemos que no funciona, así que me pongo a pensar y recuerdo la pregunta de antes. Por lo que intento probar la clave de neptuno con el nombre de la persona que descubrió Neptuno: **Gottfried**

![image](https://github.com/user-attachments/assets/74551631-d2fc-452d-b2dc-a81ea9a46c25)

## Escalada de Privilegios

Listamos todos los archivos de la carpeta de trabajo y encontramos este fichero oculto:

![image](https://github.com/user-attachments/assets/bcfd1dfb-fbbe-445f-829c-b7c827a0b872)

Miramos lo que puede ser un posible usuario, o no.

Comprobamos el /etc/passwd para ver los usuarios y vemos que hay un usuario que se llama **nasa**, tienen relación ya que EisenHower fundó la NASA. Por lo que es la clave del usuario nasa.

![image](https://github.com/user-attachments/assets/cf0ab161-6693-44cd-b0c9-37418fc67300)

Vemos que podemos ejecutar como el usuario **elite** el binario *socat*.

![image](https://github.com/user-attachments/assets/24e16a28-9df6-4c3d-a13a-bfc4eaff1c35)

Buscamos en [GTFOBins](https://gtfobins.github.io/) como explotar este binario.

![image](https://github.com/user-attachments/assets/ee474d9a-30a0-4469-b819-7c7e480b786a)

Conseguimos la shell como el usuario **elite**.

![image](https://github.com/user-attachments/assets/e81d1a87-a8b5-4ea8-ba01-f3f1a6d3118e)

Hago una reverse shell y la sanitizo para que no me de problemas, ya que aunque hagas el `script /dev/null -c bash` no funciona del todo.

Ponemos la IP nuestra (kali) y nos ponemos en escucha `nc -nlvp 443`

![image](https://github.com/user-attachments/assets/301f98d3-23f4-453a-9b61-87499813aefe)

Nos ponemos en escucha y escribimos la revshell a mano ya que si copiamos y pegamos nos salen caracteres y al principio y al final, y no se pueden borrar.

![image](https://github.com/user-attachments/assets/318432a8-908f-4d72-9cfb-1c50bed87136)

Conseguimos la shell y la sanitizamos. [Tutorial personal de cómo sanitizar TTY](https://github.com/Alv-fh/tty_treatment)

![image](https://github.com/user-attachments/assets/a9c81fa7-43f1-4193-96e4-0747cdb0fb15)

### Bypass de Contraseña

Ahora hacemos un `sudo -l` y vemos que ponemos ejecutar como el usuario root el binario **chown**.

![image](https://github.com/user-attachments/assets/15870954-ce8a-4d69-af31-a144ca7d96d4)

Bypass de Contraseña es una técnica de Escalada de Privilegios que consiste en intentar quitar la x de un usuario del /etc/passwd para poder acceder sin contraseña a él

Como vemos siempre sigue la misma sintaxis. En esta técnica vamos a fijarnos en los dos primeros campos separados por el delimitador (:)

1 -> Usuario

2 -> Referencia al /etc/shadow (Archivo donde se guardan las contraseñas, encriptadas y muy seguro)

La idea de esta técnica es intentar quitar esa X para que el sistema no encuentre esa referencia (/etc/shadow) al intentar entrar como un usuario (root) , por lo que entiende que no tiene contraseña y podemos entrar sin ella.

Lo primero de todo es asignarnos propietarios del /etc/passwd. Como somos el usuario elite, pues seguimos esta sintaxis.

![image](https://github.com/user-attachments/assets/83a19d16-d47d-44f2-b7b6-8741d60b49a8)

Nos copiamos el /etc/passwd pero vemos que no tenemos ningún editor de texto, así que lo copiamos en una nueva consola, en nuestro kali creamos un fichero.

![image](https://github.com/user-attachments/assets/81af4f20-471f-4eee-a235-451adb781629)

Creamos un fichero y le quitamos la **x** de root como hemos hablado anteriormente.

![image](https://github.com/user-attachments/assets/dcbff723-5861-4aa2-ad76-bffdbde46981)

Ahora lo mostramos, lo copiamos y hacemos lo siguiente:

`echo 'TODO_EL_ETC_PASSWD' > /etc/passwd`

![image](https://github.com/user-attachments/assets/efa6ec96-0e70-4c02-bdbb-4b6097e4808a)

Mostramos el /etc/passwd para comprobar que no tiene la x

![image](https://github.com/user-attachments/assets/40a42b74-46b1-4427-b1aa-87da3474fa7c)

Intentamos entrar como root y en teoría no nos pediría la contraseña:

![image](https://github.com/user-attachments/assets/b91ace36-bb51-4198-b835-b990b05b1573)

En teoría ya estaría acabada la máquina pero esto es un plus para ver si accedemos de verdad a la NASA.

Entramos y vemos este script, vamos a ejecutarlo haber lo que nos encontramos:

Vemos que es un QUIZ, interesantee!!

![image](https://github.com/user-attachments/assets/d8692c5a-7360-48f3-b3bc-d339dabed146)

![image](https://github.com/user-attachments/assets/ea44a5a5-d2ef-4f35-ae0c-0ad2310aad21)

Y ahora vemos el resultado:

Oleee!!!

![image](https://github.com/user-attachments/assets/f2db0e99-49a2-4794-8bac-4dea7f28df4f)

Espero que os haya gustado :)
