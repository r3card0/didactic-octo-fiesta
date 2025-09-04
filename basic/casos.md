# Proceso para instalar Python 3.8.6 y que conviva con la version 10

## `sudo apt update && sudo apt install -y\` 
El propósito de este grupo de comandos es actualizar los paquetes disponibles antes de instalar nuevos paquetes

## `sudo apt upgrade`
El propósito es actualizar la lista local de paquetes disponibles desde los repositorios configurados en mi sistema Linux.

* `sudo`: Ejecuta el comando con privilegios de administrador. Al usar este comando, lo primero que hace es solicitarme la contraseña de administrador. Asi que debo tener la contraseña disponible.
* `apt`: Es el gestor de paquetes de sistemas basados en Debian/Ubuntu
* `update`: Descarga la información mas reciente sobre qué paquetes están disponibles y sus versiones.

Esta combinación NO ❌ instala ni actualiza paquetes. Solo refresca el índice local para saber qué hay disponible.

`&&`: Es un operador lógico que ejecuta el segundo comando, solo si el primero se completó exitosamente (código de salida 0)
