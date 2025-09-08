# Nuevo
# Comando para crear un reporte de archivos

Comandos en *Terminal* que me permitan sacar un reporte de la cantidad de archivos que hay en un directorio agrupados por su extension.

El siguiente comando lo corri en WLS y  me funciona bien porque retorno un pequeño reporte en terminal de la cantidad de archivos agrupados por extensión en el directorio desktop

```bash
find -type f -name "*.*" | awk -F . '{print $NF}' | sort | uniq -c | sort -nr
```

`find` es usado para buscar archivos y con la bandera -type . . . pero veo que esta el simbolo '|', cual es su funcion? concatena los diversos grupos de comandos. Entiendo que este grupo de comandos `find -type f -name "*.*"` busca los archivos por extension sin considerar su nombre. el segundo grupo, `awk -F . '{print $NF}' `, como bien mencionaste procesa los datos del comando anterior y procesa por cada linea, usando el punto como separador, -F e imprime los caracteres que van después del punto: ¨.¨. El comando sort, ordena los datos, el comando `unique -c` los agrupa y finalmente los vuelte a ordenar `sort -nr` pero desconozco que hace la flag `-nr`

El símbolo pipe ` | `, no concatena, sino que *conecta la salida de un comando con la entrada del siguiente* Es como una tuberia que pasa datos de izquierda a derecha:

```
comando1 | comando2 | comando3
   ↓         ↓         ↓
 salida → entrada → salida
```

La bandera `-nr` en `sort`:

* `-n` ordena numéricamente (no alfabético). Ordena de menor a mayor por definición. Si `sort` no lleva `-n`, entonces ordena alfabéticamente por defaul.
* `-r` significa reverso, ordenando de mayor a menor.

Mi resumen es que esta secuencia de comandos generan salidas y con conectadas por el simbolo pipe ` | `:

* Este grupo de comandos `find -type f -name "*.*"` busca los archivos *¡QUE TENGAN EXTENSION!*, que tenga un punto y algo después, sin considerar su nombre. `*.*` Significa: cualquier nombre + punto + cualquier extensión. Si busca archivos que tengan extensión y *excluye archivos sin punto*.
* el segundo grupo, `awk -F . '{print $NF}' `, como bien mencionaste procesa los datos del comando anterior y procesa por cada linea, usando el punto como separador, -F e imprime los caracteres que van después del punto: ` . `
*  El comando `sort`, ordena los datos alfabéticamente.
*  El comando `unique -c` los agrupa y finalmente
*  Los ordena numéricamente de mayor a menor : `sort -nr`

Ejemplo práctico:

```bash
# En tu directorio tienes:
archivo.txt    ← SÍ lo encuentra (tiene punto)
foto.jpg       ← SÍ lo encuentra (tiene punto) 
README         ← NO lo encuentra (sin punto)
script.py      ← SÍ lo encuentra (tiene punto)
```

## Para uso en MAC
Es necesario agregar el punto `. `, después de `find`. Es necesario agregarlo para indicarle el directorio actual. Usanot punto se infiere que donde estas parado se ejecute el reporte.

```bash
find . -type f -name "*.*" | awk -F . '{print $NF}' | sort | uniq -c | sort -nr
```

Si estas parado en directorio raiz y quieres un reporte de otro directorio, la variante es:

```bash
find ~/Desktop -type f -name "*.*" | awk -F . '{print $NF}' | sort | uniq -c | sort -nr
```


# Crear un archivo CSV que contenga la lista de archivos de una carpeta
Crear un CSV, con la lista de archivos de una carpeta, por ejemplo **Desktop**, me permitira usarlo para manejar los archivos con Python a través de pandas.

Por ejemplo, en un proyecto requeria que todos los achivos **shapefile** fueran expuestos a una función de Python. Entonces, la funcion requeria como parametro, el path de cada archivo. Fue asi que mediante el archivo CSV podia iterar los nombres , agregarles el path comun y pasar cada uno en la funcion.

Para crear el archivo CSV con puros shapefiles usé el siguiente comando en Terminal:

```bash
ls *.sph > shapes.csv
```

El comando dice: lista `ls` todos `*` los archivos con extensión `.sph` y pásalos `>` (su nombre) al archivo `shapes.csv`

Una vez creado el archivo, fue usado en la siguiente función de Python:

```python
import pandas as pd

class GetListFiles:
    def __init__(self,filepath:str,filename:str) -> None:
        self.filepath = filepath
        self.filename = filename
        
    # returns a list of shapefiles
    def get_list_of_files(self):
        file_name = f"{self.filepath}/{self.filename}"
        # create a dataframe without headers
        df = pd.read_csv(file_name,header=None)
        # set new column
        df.columns = ["shp_file"]
        # convert to list
        files_list = list(df.archivo)
        return files_list
    
    # returns the complete path of every shapefile
    def set_complete_path(self):  
        # Create a new list appliying a list comprehension     
        final_list = [f"{self.filepath}/" + i for i in self.get_list_of_files()]        
        return final_list
```

Esta función recibe dos parámetros:

* El path del directorio en común de los archivos
* El nombre del archivo CSV del cual se extraerán los nombres de cada shapefile

La función retorna una lista de path comun + el nombre del shapefile, implementando una *[list comprehension](https://github.com/r3card0/Python-Notes/blob/main/PythonIntermediate/17_list_comprehensions.ipynb)*. Esta lista será usada en una función de Python para ejecutar una tarea.

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

# curl https://

# Asegurar que el sistema identifique pyenv
Al terminar de instalar pyenv, hacer lo siguiente:
1. Asegurar que el sistema identifique pyenv

Abri el archivo `~/.bashrc`, con **nano** -> `nano ~/.bashrc`
```bash
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

2. Guardar el archivo
3. Recargar el sistema con: `source ~/.bashrc`
4. Probar si lo reconoce el sistema con : `pyenv --version`

# Instalar la versión Python 3.8.6

```bash
cd /usr/src
sudo wget https://www.python.org/ftp/python/3.8.6/Python-3.8.6.tgz
sudo tar xzf Python-3.8.6.tgz
cd Python-3.8.6
sudo ./configure --enable-optimizations
sudo make -j$(nproc)
sudo make altinstall
```

## Instalar Python 3.8.6
1. Correr el comando `pyenv install 3.8.6`
2. Verificar la versión instalada con el comando `pyenv versions` y deberá devolver:
     ```bash
     system (set by /home/miusuario/.pyenv/version)
     3.8.6
     ```
## Crear el entorno virtual con la version 3.8.6
* Ir al directorio donde llevare el proyecto o crearlo
* Crear el entorno virtual de la version 3.8.6: `pyenv virtualenv 3.8.6  miproyecto`
* Activar el entorno virtual o que se active automaticamente `pyenv local miproyecto`
* Confirmar la version del entorno `python --version` y debe retornar `3.8.6`
* Instalar librerias con `pip install pandas`

Cada libreria quedaría solo dentro de ese entorno, no afecta mi Python 3.10 ni otros proyectos.
     
