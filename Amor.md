![1](https://github.com/user-attachments/assets/114e16f6-a9b4-49ea-992d-0820ab0e4fda)

## Conectividad con la máquina objetivo

Hacemos ping a la IP que nos da al arrancar la máquina y vemos que tenemos conectividad con la máquina.

![2](https://github.com/user-attachments/assets/4e561066-8daf-478d-a54f-93862c07b758)

## Descubrimiento de Puertos y Servicios

Utilizamos la herramienta nmap y vemos los puertos que tienen abiertos.

`sudo nmap -p- -sS -sC -sV -O 172.17.0.2 -oN puertos.txt`

![3](https://github.com/user-attachments/assets/eb8b4ac4-9654-4dca-b233-d29479175e8f)

### Puertos y Servicios abiertos

| Puerto | Servicio | Versión       |
| ------ | -------- | ------------- |
| 22     | SSH      | OpenSSH 9.6p1 |
| 80     | HTTP     | Apache 2.4.58 |

## Fase de Explotación 

Vemos que en la página web hay un nombre **Carlota** que es el del Departamento de ciberseguridad.

![4](https://github.com/user-attachments/assets/ef0d21b3-e9cc-440a-86df-19b409572559)

Intentamos hacer ataque de fuerza bruta con **hydra**

`hydra -l carlota -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -I -t 64`

![5](https://github.com/user-attachments/assets/73f020fd-522c-4e1a-af5f-d21acdc2f391)

Entramos por SSH y le hacemos el tratamiento de TTY

![6](https://github.com/user-attachments/assets/9ae81bda-361d-43ea-a93a-44472a3973d5)

Vemos que hay una imagen. Nos la traemos al Kali montando un servidor en python en la máquina objetivo.

`python3 -m http.server`

Ahora en el Kali ejecutamos:

`wget http://172.17.0.2:8000/imagen.jpg`

![7](https://github.com/user-attachments/assets/9b924e5f-0f95-44ea-9f4a-d7c198fd51a4)

Ahora la vemos con cualquier visor de imagen:

![8](https://github.com/user-attachments/assets/d33b65dc-f8ed-4ec8-87ea-5bdb99a58eaa)

Aparentemente no hay nada.

Veo que podemos ver el **/etc/passwd** y que hay otro usuario llamado **oscar**.

![9](https://github.com/user-attachments/assets/c591708b-b3fa-488e-9ff5-ba51cf71cbdc)

Vemos que hay una imagen. Nos la descargamos con un servidor de Python por ejemplo.

![10](https://github.com/user-attachments/assets/710a6ca0-244b-4656-aca3-86bebbebff94)

Ahora con la herramienta **steghide** vemos el contenido de dicha imagen y vemos que hay un **secret.txt** que parece un texto en **Base64**

![11](https://github.com/user-attachments/assets/d0ec7c44-9249-414b-835f-8225921f7bf5)

Lo convertimos en **Base64** y probamos a entrar con *Oscar*.

![12](https://github.com/user-attachments/assets/e9a31ce0-d651-4728-9791-326e6b9632a3)

Entramos y vemos que ponemos ejecutar el binario **ruby** como cualquier usuario.

![13](https://github.com/user-attachments/assets/de01ef8a-fbe4-4deb-b9a3-2d7815f9140d)

Buscamos en nuestra herramienta **Searchbins**.

![14](https://github.com/user-attachments/assets/712c1a3b-199c-4a8c-97d5-c1f7a5408ff9)


Y conseguimos root.

![15](https://github.com/user-attachments/assets/256909ef-ceb5-45c1-a351-43313887cc11)

