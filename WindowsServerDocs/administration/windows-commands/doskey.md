---
title: doskey
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e7a2fd2aa6a608a8857b325d3d6b0608eb4e59e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439502"
---
# <a name="doskey"></a>doskey



Llama Doskey.exe (qué recuperaciones de línea de comandos habían especificada previamente), edite las líneas de comandos y crea macros.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

## <a name="parameters"></a>Parámetros

|       Parámetro        |                                                                                                                          Descripción                                                                                                                           |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /reinstall       |                                                                                            Instala una nueva copia de Doskey.exe y borra el búfer de historial de comandos.                                                                                            |
|   / listsize =\<tamaño >    |                                                                                                Especifica el número máximo de comandos en el búfer de historial.                                                                                                 |
|        /macros         |                                        Muestra una lista de todos los **doskey** macros. Puede usar el símbolo de redirección ( **>** ) con **/macros** para redirigir la lista a un archivo. Se puede abreviar **/macros** a **/m**.                                         |
|      /macros:all       |                                                                                                        Muestra **doskey** macros para todos los archivos ejecutables.                                                                                                         |
|   /macros:\<ExeName>   |                                                                                             Muestra **doskey** macros para el ejecutable especificado por *ejecutable*.                                                                                              |
|        /history        |                                    Muestra todos los comandos que se almacenan en memoria. Puede usar el símbolo de redirección ( **>** ) con **/history** para redirigir la lista a un archivo. Se puede abreviar **/history** como **/h**.                                    |
|        [/insert        |                                                                                                                          /overstrike]                                                                                                                          |
|  /exename=\<ExeName>   |                                                                                        Especifica el programa (es decir, ejecutable) en el que el **doskey** macro se ejecuta.                                                                                         |
| /macrofile=\<FileName> |                                                                                              Especifica un archivo que contiene las macros que desea instalar.                                                                                               |
| \<MacroName>=[<Text>]  | Crea una macro que lleva a cabo los comandos especificados por *texto*. *Nombredelamacro* especifica el nombre que desea asignar a la macro. *Texto* especifica los comandos que desee registrar. Si *texto* se deja en blanco, *Nombredelamacro* se haya borrado los comandos asignados. |
|           /?           |                                                                                                              Muestra la ayuda en el símbolo del sistema.                                                                                                              |

## <a name="remarks"></a>Comentarios

- Utilizar Doskey.exe

  Doskey.exe siempre está disponible para programas interactivos basados en caracteres absoluto (por ejemplo, los depuradores de programa o programas de transferencia de archivos) y mantiene un búfer de historial de comandos y macros para cada programa que se inicia. No puede usar **doskey** opciones de línea de comandos desde un programa. Debe ejecutar **doskey** opciones de línea de comandos antes de iniciar un programa. Suplantan **doskey** las asignaciones de clave.
- Repetir un comando

  Para recuperar un comando, puede usar cualquiera de las claves siguientes después de iniciar Doskey.exe. Si se utiliza Doskey.exe dentro de un programa, las asignaciones de teclas de dicho programa tienen prioridad.  

  |    Key     |                              Descripción                              |
  |------------|-----------------------------------------------------------------------|
  |  FLECHA ARRIBA  |  Repite el comando que usó antes de que se muestra.  |
  | FLECHA ABAJO |  Repite el comando que usa después de la que se muestra.   |
  |  RE PÁG   |    Repite el primer comando que usó en la sesión actual.    |
  | AV PÁG  | Repite el comando más reciente que usó en la sesión actual. |


- Edición de la línea de comandos

  Con Doskey.exe, puede editar la línea de comandos. Si se utiliza Doskey.exe dentro de un programa, las asignaciones de teclas de dicho programa tendrán prioridad y algunas teclas de edición de Doskey.exe no funcionen.

  La siguiente tabla enumera **doskey** editar claves y sus funciones.  

  | Tecla o combinación de teclas |                                                                                                                                                       Descripción                                                                                                                                                       |
  |------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |       FLECHA IZQUIERDA       |                                                                                                                                      Mueve el punto de inserción de un carácter.                                                                                                                                      |
  |      FLECHA DERECHA       |                                                                                                                                    Mueve el punto de inserción un carácter hacia delante.                                                                                                                                     |
  |    CTRL+FLECHA IZQUIERDA     |                                                                                                                                        Mueve el punto de inserción de una palabra.                                                                                                                                         |
  |    CTRL+FLECHA DERECHA    |                                                                                                                                       Mueve el punto de inserción una palabra hacia delante.                                                                                                                                       |
  |          INICIO          |                                                                                                                                 Mueve el punto de inserción al principio de la línea.                                                                                                                                 |
  |          FINAL           |                                                                                                                                    Mueve el punto de inserción al final de la línea.                                                                                                                                    |
  |          ESC           |                                                                                                                                          Borra el comando de la pantalla.                                                                                                                                           |
  |           F1           |                                                                      Copia un carácter de una columna en la plantilla a la misma columna en la ventana de símbolo del sistema. (La plantilla es un búfer de memoria que contiene el último comando que escribió).                                                                       |
  |           F2           |                                                                 Busca hacia delante en la plantilla para la clave siguiente que escriba después de presiona F2. Doskey.exe inserta el texto de la plantilla, hacia arriba, pero no incluyen, el carácter especifique.                                                                  |
  |           F3           |                                                 Copia el resto de la plantilla en la línea de comandos. Doskey.exe comienza a copiar los caracteres desde la posición en la plantilla que corresponde a la posición indicada por el punto de inserción en la línea de comandos.                                                 |
  |           F4           |                                                                            Elimina que todos los caracteres desde la inserción actual punto posición hacia arriba, pero no incluyen la siguiente aparición del carácter que escriba después de presione F4.                                                                            |
  |           F5           |                                                                                                                                   Copia la plantilla en la línea de comandos.                                                                                                                                    |
  |           F6           |                                                                                                                    Coloca un carácter de final de archivo (CTRL+Z) en la posición actual del punto de inserción.                                                                                                                    |
  |           F7           | Muestra todos los comandos para este programa que se almacenan en memoria (en un cuadro de diálogo). Utilice la tecla flecha arriba y flecha abajo para seleccionar el comando que desee y presione ENTRAR para ejecutar el comando. También puede tener en cuenta el número secuencial delante del comando y use este número junto con la tecla F9. |
  |         ALT+F7         |                                                                                                                          Elimina todos los comandos que se almacenan en memoria para el búfer de historial actual.                                                                                                                          |
  |           F8           |                                                                                                           Muestra todos los comandos en el búfer de historial que comienzan con los caracteres en el comando actual.                                                                                                            |
  |           F9           |                                             Solicita un número de comandos de búfer de historial y, a continuación, muestra el comando asociado con el número que especifique. Presione ENTRAR para ejecutar el comando. Para mostrar todos los números y sus comandos asociados, presione la tecla F7.                                             |
  |        ALT+F10         |                                                                                                                                             Elimina todas las definiciones de macro.                                                                                                                                              |


- Uso de **doskey** dentro de un programa

  Ciertos programas basados en caracteres, interactivos, como los depuradores de programa o programas de transferencia de archivos (FTP) usan automáticamente Doskey.exe. Para utilizar Doskey.exe, un programa debe ser un proceso de consola y usar datos almacenados en búfer. Suplantan **doskey** las asignaciones de clave. Por ejemplo, si el programa utiliza la tecla F7 para una función, no se puede obtener un **doskey** historial en una ventana emergente de comandos.

  Con Doskey.exe, puede mantener un historial de comandos para cada programa que iniciar o repetir. Puede editar los comandos anteriores en el símbolo del sistema del programa e iniciar **doskey** macros creadas para el programa. Si sale y reinicia, a continuación, desde la misma ventana de símbolo del sistema, el historial de comandos de la sesión anterior del programa está disponible.

  Debe ejecutar Doskey.exe antes de iniciar un programa. No puede usar **doskey** las opciones de línea de comando del programa de un símbolo del sistema, aunque el programa tiene un comando de shell.

  Si desea personalizar el funcionamiento de Doskey.exe con un programa y crear **doskey** macros para que el programa, puede crear un programa por lotes que modifique Doskey.exe e inicie el programa.
- Especifica el modo de inserción predeterminado

  Si presiona la tecla INSERT, puede escribir texto en el **doskey** línea de comandos en mitad del texto existente sin reemplazar el texto. Sin embargo, después de presionar ENTRAR, Doskey.exe devuelve el teclado para el modo de reemplazo. Debe presionar Insertar nuevo para volver al modo de inserción.

  Use **/inserción** para cambiar el teclado a modo de inserción cada vez presione ENTRAR. El teclado permanece en modo de inserción hasta que use **//overstrike**. Puede devolver temporalmente al modo de reemplazo presionando la tecla Insertar, pero después de presionar ENTRAR, Doskey.exe devuelve el teclado para el modo de inserción.

  La inserción punto cambios forma al usar la tecla INSERT para cambiar de un modo a otro.
- Creación de una macro

  Puede utilizar Doskey.exe para crear macros que llevan a cabo uno o más comandos. En la tabla siguiente se enumera los caracteres especiales que puede usar para controlar las operaciones de comando cuando se define una macro.  

  |   Carácter   |                                                                                                                                                                               Descripción                                                                                                                                                                               |
  |---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |   $G o $g    |                                                                                   Redirige los resultados. Utilice cualquiera de estos caracteres especiales para enviar la salida a un dispositivo o un archivo en lugar de a la pantalla. Este carácter es equivalente al símbolo de redirección para la salida ( **>** ).                                                                                    |
  | $G$ G o $g g de $  |                                                         Anexa la salida al final de un archivo. Utilice uno de estos caracteres dobles para anexar la salida a un archivo existente en lugar de reemplazar los datos en el archivo. Estos caracteres dobles son equivalentes al símbolo de redirección para la salida de anexar ( **>>** ).                                                         |
  |   $L o $l    |                                                                                  Redirige la entrada. Use cualquiera de estos caracteres especiales para leer desde un dispositivo o un archivo en lugar de desde el teclado de entrada. Este carácter es equivalente al símbolo de redirección para la entrada ( **<** ).                                                                                  |
  |   $B o $b    |                                                                                                                                    Envía los resultados de la macro a un comando. Estos caracteres especiales son equivalentes a utilizar la canalización (\*\*                                                                                                                                     |
  |   $T o $t    |                                                            Separa los comandos. Use cualquiera de estos caracteres especiales para separar los comandos al crear macros o escriba los comandos en el **doskey** línea de comandos. Estos caracteres especiales son equivalentes a utilizar la y comercial ( **&** ) en una línea de comandos.                                                            |
  |      $$       |                                                                                                                                                              Especifica el carácter de signo de dólar ( **$** ).                                                                                                                                                               |
  | $1 a $9 |             Representa cualquier información de línea de comandos que desea especificar al ejecutar la macro. Los caracteres especiales **$1** a través de **$9** son parámetros de proceso por lotes que le permiten usar datos diferentes en la línea de comandos cada vez que ejecute la macro. El **$1** caracteres en un **doskey** comando es similar a la **%1** caracteres en un programa por lotes.             |
  |      $\*      | Representa toda la información de línea de comandos que desea especificar al escribir el nombre de macro. El carácter especial **$ \\** \* es un parámetro reemplazable que es similar a los parámetros del lote **$1** a través de **$9**, con una diferencia importante: todo lo que escribe en la línea de comandos después del nombre de macro se reemplaza por la **$ \\** \* en la macro. |


- Ejecuta un **doskey** macro

  Para ejecutar una macro, escriba el nombre de macro en el símbolo del sistema, empezando en la primera posición. Si se ha definido la macro con **$ \\** * o cualquiera de los parámetros del lote **$1** a través de **$9**, use un espacio para separar los parámetros. No se puede ejecutar un **doskey** macro a partir de un programa por lotes.
- Creación de una macro con el mismo nombre que un comando de la familia Windows Server 2003

  Si siempre usa un comando determinado con opciones de línea de comandos específicas, puede crear una macro que tenga el mismo nombre que el comando. Para especificar si desea ejecutar la macro o el comando, siga estas instrucciones:  
  -   Para ejecutar la macro, escriba el nombre de macro en el símbolo del sistema. No agregue un espacio delante del nombre de macro.
  -   Para ejecutar el comando, inserte uno o más espacios en el símbolo del sistema y, a continuación, escriba el nombre del comando.
- Eliminación de una macro

  Para eliminar una macro, escriba:  
  ```
  doskey <MacroName> =
  ```

## <a name="BKMK_examples"></a>Ejemplos

El **/macros** y **/history** opciones de línea de comandos son útiles para crear programas de batch para guardar las macros y comandos. Por ejemplo, para almacenar todos los actual **doskey** macros, escriba:
```
doskey /macros > macinit 
```
Para utilizar las macros guardadas en Macinit, escriba:
```
doskey /macrofile=macinit 
```
Para crear un lote denominado Tmp.bat que contiene recientemente el programa utiliza comandos, escriba:
```
doskey /history> tmp.bat 
```
Para definir una macro con varios comandos, utilice **$t** para separar los comandos, como se indica a continuación:
```
doskey tx=cd temp$tdir/w $*
```
En el ejemplo anterior, la macro TX cambia el directorio actual a Temp y, a continuación, muestra una lista de directorios en formato de presentación ancho. Puede usar **$ \\** * al final de la macro para anexar otras opciones de línea de comandos para **dir** cuando ejecute TX.

La siguiente macro utiliza un parámetro por lotes para un nuevo nombre de directorio:
```
doskey mc=md $1$tcd $1
```
La macro crea un nuevo directorio y, a continuación, cambia al nuevo directorio desde el directorio actual.

Para utilizar la macro anterior para crear y cambiar a un directorio denominado los libros en pantalla, escriba:
```
mc books
```
Para crear un **doskey** macro para un programa llamado Ftp.exe, incluya **/exename** como sigue:
```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye 
```
Para usar la macro anterior, inicie FTP. En el símbolo del sistema FTP, escriba:
```
go
```
Ejecuciones FTP la **abrir**, **mget**, y **bye** comandos.

Para crear una macro que rápidamente y de forma incondicional da formato a un disco, escriba:
```
doskey qf=format $1 /q /u
```
Para rápidamente y de forma incondicional formatear un disco de la unidad, escriba:
```
qf a:
```
Para eliminar una macro denominada vlist, escriba:
```
doskey vlist =
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)