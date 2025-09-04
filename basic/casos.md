# Proceso para instalar Python 3.8.6 y que conviva con la version 10

Instalación típica para preparar el entorno de desarrollo de Python🐍

`sudo apt update && sudo apt install -y \
make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev \
python3-openssl git`

## `sudo apt update && sudo apt install -y \` 
El propósito de este grupo de comandos es actualizar los paquetes disponibles antes de instalar nuevos paquetes

## `sudo apt upgrade`
El propósito es actualizar la lista local de paquetes disponibles desde los repositorios configurados en mi sistema Linux.

* `sudo`: Ejecuta el comando con privilegios de administrador. Al usar este comando, lo primero que hace es solicitarme la contraseña de administrador. Asi que debo tener la contraseña disponible.
* `apt`: Es el gestor de paquetes de sistemas basados en Debian/Ubuntu
* `update`: Descarga la información mas reciente sobre qué paquetes están disponibles y sus versiones.

Esta combinación NO ❌ instala ni actualiza paquetes. Solo refresca el índice local para saber qué hay disponible.

## `&&`
`&&`: Es un operador lógico que su propósito es ejecutar el segundo comando, ***solo si***, el primero se completó exitosamente (código de salida 0).

Si `apt update` falla, el sistema no continuará con la instalación.

## `sudo apt install -y \`
El propósito es instalar paquetes que yo especifique.
* `install`: Comando para instalar paquetes
* `-y`: Es un `flag` que responde automáticamente **yes** a todas las preguntas de confirmación.
* `\`: Caracter de escape que permite continuar el comando en la siguiente línea.

## Propósito general del conjunto de comandos
Esta es una secuencia de comandos muy común en scripts y tutoriales porque:
1. ✅**Garantizar informacion actualizada**, primero actualizando el índice de paquetes.
2. 🔒**Instalar de forma segura**, y que solo procede si la actualizacion previa fue exitosa.
3. ⚙️**Automatizar la instalación**, y el flag `-y` evita interrupciones por confirmaciones.
4. 🛣️**Permitir múltiples paquetes**, usando `\` al final indica que seguirán más paquetes en las siguientes líneas.

Es una práctica recomendada ejecutar `apt update` antes de instalar paquetes para asegurar que se descarguen las versiones mas recientes disponibles.

## `make`
El propósito de `make` es automatizar ⚙️ la compilación.
* Ejecuta instrucciones definidas en archivos `Makefile`
* Coordina la compilación de proyectos complejos
* Determina automáticamente qué partes del programa necesitan recompilarse
* Esencial para compilar software desde código fuente

## `build-essential`
Es un meta-paquete que contiene las herramientas básicas para compilar.
