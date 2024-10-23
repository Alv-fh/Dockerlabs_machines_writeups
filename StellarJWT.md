Hacemos los escaneo de reconocimiento y vemos el puerto 22 y 80 abiertos

![2](https://github.com/user-attachments/assets/4df23b27-982f-4f04-8b54-c6b6901de8c3)

Entramos en la Web y vemos esta pregunta

![3](https://github.com/user-attachments/assets/211a5138-2797-4d97-9368-25224195b68f)

Buscamos en Internet y nos quedamos con el nombre Gottfried:

![4](https://github.com/user-attachments/assets/77710fbe-122f-42a6-9fe6-c2065b2a7e9c)

Hacemos fuzzing y vemos una ruta:

/universe

![5](https://github.com/user-attachments/assets/66fb12e9-d19a-489a-a8fd-b84f58c4ff5f)

Vemos una imagen que a simple vista no hay nada.

![6](https://github.com/user-attachments/assets/4549eddb-3f15-4548-8c5b-e7a7d6c316a1)

Entramos en el código fuente y si bajamos abajo del todo vemos lo que parece un JSON.

![7](https://github.com/user-attachments/assets/2f64a15e-b2df-4166-9adc-b67442c2cb2b)

Entramos en JWT y descubrimos el usuario neptuno.

![8](https://github.com/user-attachments/assets/d3041b4d-3c5e-4bbe-bd4e-f6e0d1191e77)

Ahora la mayoría de gente intentará fuerza bruta, pero recordamos el nombre de antes, así que conseguimos entrar por ssh con la clave Gottfried.

Listamos todo y vemos un archivo oculto.

![9](https://github.com/user-attachments/assets/db9c95f8-7dec-4316-b316-a6ca970bed80)

Lo abrimos y a simple vista parece un usuario. Vemos los usuarios del sistema y vemos uno que es nasa, intentamos poner de clave el nombre ese y efectivamente entramos.

![10](https://github.com/user-attachments/assets/0ad30d8f-7937-4ca4-9399-bacb1800c94e)

Vemos que podemos ejecutar el binario socat como el usuario elite. Lo ejecutamos buscando en GTFOBins y conseguimos la shell. Aquí recomiendo que hagan una revshell porque luego va a dar problemas el Bypass de contraseña.

![11](https://github.com/user-attachments/assets/4ac29980-3cb4-4014-bbc9-b25c661267a1)

Hacemos una revshell y sanitizamos la TTY.

![12](https://github.com/user-attachments/assets/d737ce40-ac65-46d6-99b4-947daa8ec8d7)

Ahora sanitizamos.

![13](https://github.com/user-attachments/assets/44f65ceb-76fc-45dc-bf77-a0b22ef22c5b)

Hacemos sudo -l y vemos que podemos ejecutar el binario chown como root.

Bypass de contraseña entra aquí

Nos asignamos propietarios del /etc/passwd

![14](https://github.com/user-attachments/assets/d5163122-29bf-4815-b6af-5ab0e1981bab)

No tenemos editor de texto instalado, por lo que hacemos lo siguiente

Copiamos el /etc/passwd y en nuestro Kali lo ponemos en un fichero y le quitamos la x

![15](https://github.com/user-attachments/assets/3676f0cf-4392-4cf2-bb11-c3b780d8100f)

Ahora lo mostramos y lo copiamos.

Ponemos 
`echo 'TODO_EL_ETC_PASSWD' > /etc/passwd`

Ya no tiene la x root

![16](https://github.com/user-attachments/assets/1622113c-9670-4e4a-a756-3d416400c2a8)

Entramos y nos vamos a su directorio y vemos un script. Lo ejecutamos y ...

![17](https://github.com/user-attachments/assets/3f1b788d-9062-4404-b01c-02b13c611c1a)

![18](https://github.com/user-attachments/assets/2ba61a12-5dc4-4b77-b562-8b56eabff14e)

![19](https://github.com/user-attachments/assets/24c423b3-1f73-4318-98cb-859ea8b48896)

Es un test, para ver si se ha aprendido lo que se ve en esta máquina con algunas bromas :)

Espero que te guste Don Mario.
