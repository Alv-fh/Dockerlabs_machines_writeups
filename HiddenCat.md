# HiddenCat

![[Writeups/Images/HiddenCat/1.png]]

## Conectividad con la máquina objetivo

Comprobamos que tenemos conectividad y que el TTL es de 64, por lo que se trata de una máquina Linux.

![[Writeups/Images/HiddenCat/2.png]]

## Escaneo de Puertos y Servicios

Hacemos un escaneo simple con nmap para averigurar que puertos y servicios corren, también descubriremos la versión.

![[Writeups/Images/HiddenCat/3.png]]

| Puerto | Servicio      | Versión |
| ------ | ------------- | ------- |
| 22     | SSH           | 7.9p1   |
| 8009   | Apache Jserv  | 1.3     |
| 8080   | Apache Tomcat | 9.0.30  |

Buscamos en el Navegador y aparece esto:

![[Writeups/Images/HiddenCat/4.png]]

Busco en **Metasploit** el servicio y la versión y encuentro este exploit que es el que mejor pinta tiene. En un entorno real probariamos uno a uno a no ser que filtremos por versión.

![[Writeups/Images/HiddenCat/6.png]]

Vemos lo que nos pide.

![[Writeups/Images/HiddenCat/7.png]]

Ponemos en:

**RHOSTS -> IP objetivo**
**RPORT -> 8009**

![[Writeups/Images/HiddenCat/8.png]]

**run** y vemos que hay un mensaje de bienvenida, en el que hay un usuario.  **jerry**.
![[Pasted image 20240624172529.png]]


Utilizamos hydra para hacer un ataque de fuerza bruta por SSH ya que ese puerto lo tenemos abierto.

`hydra -l jerry -P /usr/share/wordlist/rockyou.txt ssh://172.17.0.2 -t 64 -I`

![[Writeups/Images/HiddenCat/10.png]]

Vemos que la clave es *chocolate*. Un clásico del Pingüino de Mario, ajajaja. Entramos y buscamos para poder ser root.

![[Writeups/Images/HiddenCat/11.png]]

Vemos que podemos ejecutar con permisos SUID el binario python. Por lo que buscamos en nuestra herramienta [searchbins](https://github.com/r1vs3c/searchbins).

![[Writeups/Images/HiddenCat/12.png]]

Entonces lo ejecutamos en la máquina y...

![[Writeups/Images/HiddenCat/13.png]]

Ya somos root!!
