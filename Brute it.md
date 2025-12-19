
# Reconocimiento 

![](images/Pasted%20image%2020251218195826.png)

![](images/Pasted%20image%2020251218200348.png)

tenemos la pagina de apache default, vamos a ver que directorios ocultos tiene

![](images/Pasted%20image%2020251218200419.png)

vemos que tiene un admin, accedamos a el

![](images/Pasted%20image%2020251218200501.png)

tenemos un login form basico de toda la vida

![](images/Pasted%20image%2020251218200613.png)
tenemos esto en el codigo fuente de el login y vemos que dice que el username es admin asi que podemos intentar usar fuerza bruta para descifrar la contrasena

![](images/Pasted%20image%2020251218201823.png)

ya tenemos la contrasena con este comando de hydra

Explicacion del comando
### Desglose paso a paso:

- **`-l admin`**: Le indicamos el usuario fijo (gracias a la pista del comentario HTML en la línea 29: _"the username is admin"_).
    
- **`-P /usr/share/wordlists/rockyou.txt`**: Usamos el diccionario estándar para buscar la contraseña.
    
- **`http-post-form`**: El módulo para atacar formularios web.
    
- **La cadena entre comillas**:
    
    - `/admin/`: Es la ruta del archivo.
        
    - `:user=^USER^&pass=^PASS^`: Aquí está la magia. Hemos puesto `user` y `pass` porque así se llaman en tu código HTML. Hydra sustituirá `^USER^` y `^PASS^` automáticamente.
        
    - `:Username or password invalid`: La cadena que identifica el fallo.


![](images/Pasted%20image%2020251218202239.png)

tenemos esto en el panel de admin, tenemos una flag que es falsa y un id_rsa para conectarnos via ssh

![](images/Pasted%20image%2020251218202716.png)
Yo me pase el RSA Private Key File a mi maquina y en primer lugar vemos que esta encriptada, asi que haremos uso de la herramienta john the ripper para crackearla 

![](images/Pasted%20image%2020251218202830.png)
primeros hasheamos el archivo para que john pueda leerlo 

![](images/Pasted%20image%2020251218202930.png)
Y luego la crackeamos con john y el rockyou , le especifiacamos el path del wordlist y luego le especificamos el archivo hasheado anteriormente

![](images/Pasted%20image%2020251218203059.png)
y listo, con esa contrasena que encontramos ya nos podemos conectar a ssh 

Ahora viene la escalada de privilegios

![](images/Pasted%20image%2020251218203709.png)

aqui vemos que podemos usar el binario cat como root sin ofrecer contrasena, eso es una buena via para poder leer la contrasena de root y poder conectarnos facilmente

como las contrasenas estan almacenadas en /etc/shadow podemos leerla porque tenemos permisos asi que vamos a ver

![](images/Pasted%20image%2020251218204757.png)

efectivamente, ahi arriba tenemos la contrasena de root pero esta encryptada, vamos a crackearla con john

- Lo primero que hice fue crear un archivo, que en mi caso lo llame hash_root.txt y ahi meti todo esto:
 
```bash
root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.:18490:0:99999:7:::
```

luego con john hice lo mismo que la contrasena ssh

![](images/Pasted%20image%2020251218205051.png)

y ahi tenemos la contrasena para root, vamos a probarla

![](images/Pasted%20image%2020251218205128.png)
Y listo ya somos root. PWNED

![](images/Pasted%20image%2020251218205158.png)
Ahi esta la flag











