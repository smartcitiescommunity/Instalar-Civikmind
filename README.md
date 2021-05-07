# Instalar Civikmind


## Instalar en Ubuntu 20.04 LTS:

## Introdución

Cuando se crea por primera vez un Servidor Ubuntu 20.04, se debe realizar algunos pasos de configuración que son importantes pues son parte de la configuración básica. Estos pasos aumentarán la seguridad y usabilidad de tu servidor y te brindarán una base sólida para acciones posteriores.

## Paso 1: iniciar sesión como root

Para iniciar sesión en su servidor, deberá conocer la dirección IP pública de su servidor. También necesitará la contraseña o, si instaló una clave SSH para la autenticación, la clave privada de la cuenta del usuario raíz. Si aún no ha iniciado sesión en su servidor, es posible que desee seguir nuestra guía sobre cómo conectarse a Droplets con SSH, que cubre este proceso en detalle.

Si aún no está conectado a su servidor, inicie sesión ahora como usuario root usando el siguiente comando (sustituya la parte resaltada del comando con la dirección IP pública de su servidor):

```ssh root@mi_direccion_ip```
 
Acepte la advertencia sobre la autenticidad del host si aparece. Si está usando autenticación de contraseña, proporcione su contraseña de root para iniciar sesión. Si está usando una clave SSH que está protegida con contraseña, es posible que se le solicite que ingrese la contraseña la primera vez que use la clave en cada sesión. Si es la primera vez que inicia sesión en el servidor con una contraseña, es posible que también se le solicite que cambie la contraseña de root.

Sobre root
El usuario root es el usuario administrativo en un entorno Linux que tiene privilegios muy amplios. Debido a los mayores privilegios de la cuenta raíz, no se le recomienda usarla de forma regular. Esto se debe a que parte del poder inherente a la cuenta raíz es la capacidad de realizar cambios muy destructivos, incluso por accidente.

El siguiente paso es configurar una nueva cuenta de usuario con privilegios reducidos para el uso diario. Más adelante, le enseñaremos cómo obtener mayores privilegios solo en los momentos en que los necesite.


## Paso 2: creación de un nuevo usuario
Una vez que haya iniciado sesión como root, estamos preparados para agregar la nueva cuenta de usuario. En el futuro, iniciaremos sesión con esta nueva cuenta en lugar de root.

Este ejemplo crea un nuevo usuario llamado sammy, pero debes reemplazarlo con un nombre de usuario que te guste:

```adduser civikmind```
 
Se le harán algunas preguntas, comenzando con la contraseña de la cuenta.

Ingrese una contraseña segura y, opcionalmente, complete cualquier información adicional si lo desea. Esto no es obligatorio y puede presionar ENTER en cualquier campo que desee omitir.


## Paso 3: concesión de privilegios administrativos
Ahora, tenemos una nueva cuenta de usuario con privilegios de cuenta regulares. Sin embargo, es posible que a veces necesitemos realizar tareas administrativas.

Para evitar tener que cerrar la sesión de nuestro usuario normal y volver a iniciarla como la cuenta root, podemos configurar lo que se conoce como privilegios de superusuario o root para nuestra cuenta normal. Esto permitirá a nuestro usuario normal ejecutar comandos con privilegios administrativos poniendo la palabra sudo antes de cada comando.

Para agregar estos privilegios a nuestro nuevo usuario, necesitamos agregar el usuario al grupo sudo. De forma predeterminada, en Ubuntu 20.04, los usuarios que son miembros del grupo sudo pueden usar el comando sudo.

Como root, ejecute este comando para agregar su nuevo usuario al grupo sudo (sustituya el nombre de usuario resaltado con su nuevo usuario):

```usermod -aG sudo civikmind```
 
Ahora, cuando inicie sesión como su usuario habitual, puede escribir sudo antes de los comandos para realizar acciones con privilegios de superusuario.

## Paso 4: configuración de un cortafuegos básico
Los servidores Ubuntu 20.04 pueden usar el firewall UFW para asegurarse de que solo se permitan conexiones a ciertos servicios. Podemos configurar un firewall básico muy fácilmente usando esta aplicación.

Nota: Si sus servidores se ejecutan en un sistema cloud, opcionalmente puede usar el Firewalls que tienen incluido en lugar del firewall UFW. Recomendamos usar solo un firewall a la vez para evitar reglas conflictivas que pueden ser difíciles de depurar.

Las aplicaciones pueden registrar sus perfiles con UFW tras la instalación. Estos perfiles permiten a UFW administrar estas aplicaciones por nombre. OpenSSH, el servicio que ahora nos permite conectarnos a nuestro servidor, tiene un perfil registrado en UFW.

Puede ver esto escribiendo:

```ufw app list```
 
```Output
Available applications:
  OpenSSH```
```

Necesitamos asegurarnos de que el firewall permita conexiones SSH para que podamos volver a iniciar sesión la próxima vez. Podemos permitir estas conexiones escribiendo:

```ufw allow OpenSSH```
 
Luego, podemos habilitar el firewall escribiendo:

```ufw enable```
 
Escriba y y presione ENTER para continuar. Puede ver que las conexiones SSH todavía están permitidas escribiendo:

```ufw status

Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
```

## Paso 5: Habilitación del acceso externo para su usuario habitual
Ahora que tenemos un usuario habitual para el uso diario, debemos asegurarnos de poder acceder directamente a la cuenta mediante SSH.

Nota: Hasta que verifique que puede iniciar sesión y usar sudo con su nuevo usuario, le recomendamos que permanezca conectado como root. De esta manera, si tiene problemas, puede solucionar problemas y realizar los cambios necesarios como root. Si está utilizando una instancia y experimenta problemas con su conexión SSH raíz, puede iniciar sesión en la instancia usando la Consola de de su proveedor.

El proceso para configurar el acceso SSH para su nuevo usuario depende de si la cuenta raíz de su servidor utiliza una contraseña o claves SSH para la autenticación.

Si la cuenta raíz utiliza autenticación de contraseña
Si inició sesión en su cuenta raíz con una contraseña, la autenticación de contraseña está habilitada para SSH. Puede SSH a su nueva cuenta de usuario abriendo una nueva sesión de terminal y usando SSH con su nuevo nombre de usuario:

```ssh civikmind@mi_dir_ip```
 
Después de ingresar la contraseña de su usuario habitual, iniciará sesión. Recuerde, si necesita ejecutar un comando con privilegios administrativos, escriba sudo antes de este de esta manera:

```sudo command_to_run```
 
Se le pedirá su contraseña de usuario habitual cuando utilice sudo por primera vez en cada sesión (y periódicamente después).

Para mejorar la seguridad de su servidor, recomendamos encarecidamente configurar claves SSH en lugar de utilizar la autenticación de contraseña.

Si la cuenta raíz utiliza autenticación de clave SSH
Si inició sesión en su cuenta raíz con claves SSH, la autenticación de contraseña está deshabilitada para SSH. Deberá agregar una copia de su clave pública local al archivo ```~ /.ssh/``` authorised_keys del nuevo usuario para iniciar sesión correctamente.

Dado que su clave pública ya está en el archivo ```~ /.ssh/``` allowed_keys de la cuenta raíz en el servidor, podemos copiar ese archivo y estructura de directorio a nuestra nueva cuenta de usuario en nuestra sesión existente.

La forma más sencilla de copiar los archivos con la propiedad y los permisos correctos es con el comando rsync. Esto copiará el directorio .ssh del usuario raíz, conservará los permisos y modificará los propietarios del archivo, todo en un solo comando. Asegúrese de cambiar las partes resaltadas del comando a continuación para que coincida con el nombre de su usuario habitual:

```rsync --archive --chown=civikmind:civikmind ~/.ssh /home/civikmind``` 

Ahora, abra una nueva sesión de terminal en su máquina local y use SSH con su nuevo nombre de usuario:

``` ssh civikmind@mi_dir_ip``` 
 
Debe iniciar sesión en la nueva cuenta de usuario sin utilizar una contraseña. Recuerde, si necesita ejecutar un comando con privilegios administrativos, escriba sudo antes así:

``` sudo comando_ejecutar``` 
 
Se le pedirá su contraseña de usuario habitual cuando utilice sudo por primera vez en cada sesión (y periódicamente después).

## Paso 6 Adecuación de entorno

Introducción
Una entorno "LAMP" es un grupo de software de código abierto que normalmente se instala junto para permitir que un servidor aloje sitios web dinámicos y aplicaciones web escritas en PHP. Este término es un acrónimo que representa el sistema operativo Linux, con el servidor web Apache. Los datos del sitio se almacenan en una base de datos MySQL y PHP procesa el contenido dinámico.

En esta guía, instalaremos una entorno LAMP en un servidor Ubuntu 20.04 para esto prepararemos el entorno:

```sudo apt update && sudo apt-get upgrade```

## Paso 7: instalación de Apache y actualización del cortafuegos o Firewall
El servidor web Apache se encuentra entre los servidores web más populares del mundo. Está bien documentado, tiene una comunidad activa de usuarios y se ha utilizado ampliamente durante gran parte de la historia de la web, lo que la convierte en una excelente opción predeterminada para alojar un sitio web.

Instale Apache usando el administrador de paquetes de Ubuntu, ```apt```:

```sudo apt install apache2```


Si es la primera vez que usa ```sudo``` en esta sesión, se le pedirá que proporcione su contraseña de usuario para confirmar que tiene los privilegios adecuados para administrar los paquetes del sistema con ```apt```. También se le pedirá que confirme la instalación de Apache presionando Y, luego ENTER.

Una vez finalizada la instalación, deberá ajustar la configuración de su firewall para permitir el tráfico HTTP. **UFW** tiene diferentes perfiles de aplicación que puede aprovechar para lograrlo. Para enumerar todos los perfiles de aplicaciones UFW disponibles actualmente, puede ejecutar:

```sudo ufw app list```

Verá un resultado como este:
```
Output
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```

Esto es lo que significa cada uno de estos perfiles:

Apache: este perfil solo abre el puerto 80 (tráfico web normal no cifrado).
Apache Full: este perfil abre el puerto 80 (tráfico web normal sin cifrar) y el puerto 443 (tráfico cifrado TLS / SSL).
Apache Secure: este perfil solo abre el puerto 443 (tráfico cifrado TLS / SSL).
Por ahora, es mejor permitir solo conexiones en el puerto 80, ya que esta es una instalación nueva de Apache y aún no tiene un certificado TLS / SSL configurado para permitir el tráfico HTTPS en su servidor.

Para permitir solo el tráfico en el puerto 80, use el perfil de Apache:

```sudo ufw allow in "Apache"```

Puede verificar el cambio con:

```sudo ufw status```

salida:

```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                                
Apache                     ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)                    
Apache (v6)                ALLOW       Anywhere (v6)     
```

El tráfico en el puerto 80 ahora está permitido a través del firewall.

Puede hacer una verificación al azar de inmediato para verificar que todo salió según lo planeado visitando la dirección IP pública de su servidor en su navegador web (consulte cuál es su dirección IP pública si no tiene esta información ya y dirijase a ella):

Verá la página web predeterminada de Ubuntu 20.04 Apache, que está allí con fines informativos y de prueba. Debería verse algo como esto:

![Apache 2](https://assets.digitalocean.com/articles/how-to-install-lamp-ubuntu-18/small_apache_default_1804.png)

Si ve esta página, entonces su servidor web ahora está correctamente instalado y accesible a través de su firewall.

Cómo encontrar la dirección IP pública de su servidor
Si no sabe cuál es la dirección IP pública de su servidor, hay varias formas de encontrarla. Por lo general, esta es la dirección que usa para conectarse a su servidor a través de SSH.

Hay algunas formas diferentes de hacer esto desde la línea de comandos. Primero, puede usar las herramientas iproute2 para obtener su dirección IP escribiendo esto:

```ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'```

Esto le devolverá dos o tres líneas. Todas son direcciones correctas, pero es posible que su computadora solo pueda usar una de ellas, así que siéntase libre de probar cada una.

Un método alternativo es usar la utilidad curl para contactar a un tercero y decirle cómo ve su servidor. Esto se hace preguntando a un servidor específico cuál es su dirección IP:

```curl http://icanhazip.com```


