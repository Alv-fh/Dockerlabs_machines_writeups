## Levantar la máquina

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/161e9de4-fcc2-4917-b2fa-d8cf5923bec7)

### Conectividad con la máquina objetivo

Hay conectividad y vemos que el TTL es **64**, por lo que es una máquina Linux.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/35b7b3f7-979e-4ce3-b618-73f48493eb07)

## Escaneo de puertos

Utilizamos el siguiente comando para saber los puertos abiertos y las versiones:

`sudo nmap -p- --min-rate=5000 -sS -sC -sV -n -vvv 172.17.0.2 -oN todos`

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/053accbb-6f6b-4fa0-80b1-069fa2a77b89)

Vemos esto en el navegador por lo que vamos a hacerle caso.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/56239eb8-7e36-4c32-822e-a77ef0b9dbe1)

`tac /usr/share/wordlist/rockyou.txt > newrockyou.txt`

Ahora eliminamos los caracteres raros que salen y lo dejamos así.

![image](https://github.com/Alv-fh/Dockerlabs_machines_writeups/assets/109484163/24069c43-9685-43ca-918c-a79aebdad249)

Utilizamos **hydra** para hacer ataque de diccionario.

