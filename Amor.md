![[Pentesting/Writeups/Images/Amor/1.png]]

## Conectividad con la máquina objetivo

Hacemos ping a la IP que nos da al arrancar la máquina y vemos que tenemos conectividad con la máquina.

![[Pentesting/Writeups/Images/Amor/2.png]]

## Descubrimiento de Puertos y Servicios

Utilizamos la herramienta nmap y vemos los puertos que tienen abiertos.

`sudo nmap -p- -sS -sC -sV -O 172.17.0.2 -oN puertos.txt`

![[Pentesting/Writeups/Images/Amor/3.png]]

### Puertos y Servicios abiertos

| Puerto | Servicio | Versión       |
| ------ | -------- | ------------- |
| 22     | SSH      | OpenSSH 9.6p1 |
| 80     | HTTP     | Apache 2.4.58 |

## Fase de Explotación 

Vemos que en la página web hay un nombre **Carlota** que es el del Departamento de ciberseguridad.

![[Pentesting/Writeups/Images/Amor/4.png]]

Intentamos hacer ataque de fuerza bruta con **hydra**

`hydra -l carlota -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -I -t 64`

![[Pentesting/Writeups/Images/Amor/5.png]]

Entramos por SSH y le hacemos el tratamiento de TTY

![[Pentesting/Writeups/Images/Amor/6.png]]

Vemos que hay una imagen. Nos la traemos al Kali montando un servidor en python en la máquina objetivo.

`python3 -m http.server`

Ahora en el Kali ejecutamos:

`wget http://172.17.0.2:8000/imagen.jpg`

![[Pentesting/Writeups/Images/Amor/7.png]]

Ahora la vemos con cualquier visor de imagen:

![[Pentesting/Writeups/Images/Amor/8.png]]

Aparentemente no hay nada.

Veo que podemos ver el **/etc/passwd** y que hay otro usuario llamado **oscar**.

![[Pentesting/Writeups/Images/Amor/9.png]]

Vemos que hay una imagen. Nos la descargamos con un servidor de Python por ejemplo.

![[Pentesting/Writeups/Images/Amor/10.png]]

Ahora con la herramienta **steghide** vemos el contenido de dicha imagen y vemos que hay un **secret.txt** que parece un texto en **Base64**

![[Pentesting/Writeups/Images/Amor/11.png]]

Lo convertimos en **Base64** y probamos a entrar con *Oscar*.

![[Pentesting/Writeups/Images/Amor/12.png]]

Entramos y vemos que ponemos ejecutar el binario **ruby** como cualquier usuario.

![[Pentesting/Writeups/Images/Amor/13.png]]

Buscamos en nuestra herramienta **Searchbins**.

![[Pentesting/Writeups/Images/Amor/14.png]]


Y conseguimos root.

![[Pentesting/Writeups/Images/Amor/15.png]]

