# Instalar-Civikmind


# Instalar en Ubuntu 20.04 LTS:

## Introdución

Cuando se crea por primera vez un Servidor Ubuntu 20.04, se debe realizar algunos pasos de configuración que son importantes pues son parte de la configuración básica. Estos pasos aumentarán la seguridad y usabilidad de tu servidor y te brindarán una base sólida para acciones posteriores.

## Paso 1: iniciar sesión como root

Para iniciar sesión en su servidor, deberá conocer la dirección IP pública de su servidor. También necesitará la contraseña o, si instaló una clave SSH para la autenticación, la clave privada de la cuenta del usuario raíz. Si aún no ha iniciado sesión en su servidor, es posible que desee seguir nuestra guía sobre cómo conectarse a Droplets con SSH, que cubre este proceso en detalle.

Si aún no está conectado a su servidor, inicie sesión ahora como usuario root usando el siguiente comando (sustituya la parte resaltada del comando con la dirección IP pública de su servidor):

```ssh root@mi_direccion_ip```
 
Acepte la advertencia sobre la autenticidad del host si aparece. Si está usando autenticación de contraseña, proporcione su contraseña de root para iniciar sesión. Si está usando una clave SSH que está protegida con contraseña, es posible que se le solicite que ingrese la contraseña la primera vez que use la clave en cada sesión. Si es la primera vez que inicia sesión en el servidor con una contraseña, es posible que también se le solicite que cambie la contraseña de root.
