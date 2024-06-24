# HiddenCat

![1](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/ba6841a5-17c7-44df-a7c4-e60acecf1582)

## Conectividad con la máquina objetivo

Comprobamos que tenemos conectividad y que el TTL es de 64, por lo que se trata de una máquina Linux.

![2](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/7ec8cd73-86db-42bb-8c45-dbc7e09e9d5e)

## Escaneo de Puertos y Servicios

Hacemos un escaneo simple con nmap para averigurar que puertos y servicios corren, también descubriremos la versión.

![3](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/6385186e-7d66-4c6a-98c1-d0b0f517230d)

| Puerto | Servicio      | Versión |
| ------ | ------------- | ------- |
| 22     | SSH           | 7.9p1   |
| 8009   | Apache Jserv  | 1.3     |
| 8080   | Apache Tomcat | 9.0.30  |

Buscamos en el Navegador y aparece esto:

![4](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/9b94b198-76c4-455b-86f3-fdb44840a9a8)

Busco en **Metasploit** el servicio y la versión y encuentro este exploit que es el que mejor pinta tiene. En un entorno real probariamos uno a uno a no ser que filtremos por versión.

![6](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/50bae2f8-de87-4dcf-a8fd-682f843102de)

Vemos lo que nos pide.

![7](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/d92f6b36-ecb2-4377-aa99-85666ab9305a)

Ponemos en:

**RHOSTS -> IP objetivo**
**RPORT -> 8009**

![8](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/3c33a5b2-71ea-43b0-a96b-86ebdabe9abc)

**run** y vemos que hay un mensaje de bienvenida, en el que hay un usuario.  **jerry**.
![9](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/026fcd9c-8d46-42fd-bcbc-05b2d5f595f7)


Utilizamos hydra para hacer un ataque de fuerza bruta por SSH ya que ese puerto lo tenemos abierto.

`hydra -l jerry -P /usr/share/wordlist/rockyou.txt ssh://172.17.0.2 -t 64 -I`

![10](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f3812a18-2741-41bd-bcc7-56096a8fb318)

Vemos que la clave es *chocolate*. Un clásico del Pingüino de Mario, ajajaja. Entramos y buscamos para poder ser root.

![11](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/19475fc0-96e9-45ee-8f99-8285ad9e0fb2)

Vemos que podemos ejecutar con permisos SUID el binario python. Por lo que buscamos en nuestra herramienta [searchbins](https://github.com/r1vs3c/searchbins).

![12](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b3ed5568-772f-41fd-90d6-c774c8776055)

Entonces lo ejecutamos en la máquina y...

![13](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b61bffdd-0710-44c9-ba20-6503c020a2dd)

Ya somos root!!
