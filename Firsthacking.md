## Firsthacking

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/b256d721-fbed-465d-88e4-2de1d706a657)

## Conectividad con la máquina objetivo

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/d5ebd941-7cfc-4b21-9732-0680474452a1)

## Escaneo de puertos

Utilizamos la herramienta **nmap** y vemos los puertos abiertos.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/a56da5a3-09a0-47b5-a3ae-434741c2a88d)

21 -> FTP

Intento entrar como anonymous pero veo que no puedo, por lo que me decanto por la versión. La busco en internet y veo que es vulnerable. Por lo que busco un exploit con **metasploit**

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/0f48f789-3389-4f35-8640-3d22d1befe8e)

Veo lo que me pide y lo completo.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/83e40462-2d8d-4c6e-b2a1-b7957511986e)

Y ejecuto el exploit com `run`.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/54200870-f7b9-4dc1-9ca2-b1f796b52a26)

Ya somos root!!
