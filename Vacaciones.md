## Vacaciones

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/0f957329-b9f8-45de-b7a3-360ebff019cc)

 ## Conectividad con la máquina objetivo

 Comprobamos que sí tenemos conectividad con la máquina y que el **TTL** es 64 por lo que hablamos de una máquina Linux.

 ![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b83a7251-0fbd-481d-85d3-4a5f256a4360)

## Escaneo de puertos

Utilizamos la herramienta **nmap** y vemos los puertos abiertos.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/cfa2d345-916a-425c-8099-4284ca56d802)

22 -> SSH

80 -> HTTP

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/7eb41302-02f8-461a-88d0-91d052e58096)

Veo al inspeccionar la página esto por lo que pueden ser usuarios. Vamos a probar con hydra.

Encontramos la clave de **camilo**

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b00f73cc-9789-430f-8878-714d464a41ec)

Consigo entrar y consigo una bash con el comando.

`script /dev/null -c bash`

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/1ca70a61-207a-49e3-9ea9-3805906e3985)

Antes vimos que ponía que nos había dejado un correo por lo que voy a buscar algún archivo .txt y encuentro el siguiente:

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/8c43c0d2-ef92-42b5-8933-4d83c0e8e805)

Por lo que decido verlo y sale esto:

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/f989795b-e4cf-4bd1-a61a-a5515ce3c55c)

Entro como Juan y vuelvo a conseguir la bash.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/cab6d1df-7abf-4f43-ab17-5141a39f2d64)

Veo que puedo ejecutar como cualquier usuario el binario **ruby**

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/7441ca26-1eb8-4bc6-9d32-51a0f83b651e)

Busco en mi herramienta **searchbins** y me aparece esto.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/4d0ad93d-322d-48ed-9741-1d6f4f663c08)

Lo ejecutamos así para que no hay problemas ya que forzamos que lo ejecute como root, además debemos poner la ruta absoluta del binario.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/6959481d-f28d-4e1c-9124-4ff2668899ed)

Ya somos root!!
