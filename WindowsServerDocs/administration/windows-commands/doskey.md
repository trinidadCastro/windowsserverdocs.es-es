---
title: doskey
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fca89bd2e99b6139b13a5bd45481ae0ec2248574
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720806"
---
# <a name="doskey"></a>doskey



Llama a Doskey. exe (que recupera los comandos de línea de comandos previamente especificados), edita las líneas de comandos y crea macros.



## <a name="syntax"></a>Sintaxis

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

### <a name="parameters"></a>Parámetros

|       Parámetro        |                                                                                                                          Descripción                                                                                                                           |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /REINSTALL       |                                                                                            Instala una nueva copia de Doskey. exe y borra el búfer del historial de comandos.                                                                                            |
|   /LISTSIZE =\<tamaño>    |                                                                                                Especifica el número máximo de comandos en el búfer del historial.                                                                                                 |
|        /macros         |                                        Muestra una lista de todas las macros de **doskey** . Puede utilizar el símbolo de redirección (**>**) con **/macros** para redirigir la lista a un archivo. Puede abreviar **/macros** para **/m**.                                         |
|      /macros: ALL       |                                                                                                        Muestra macros de **doskey** para todos los ejecutables.                                                                                                         |
|   /macros:\<ExeName>   |                                                                                             Muestra las macros de **doskey** para el ejecutable especificado por *ExeName*.                                                                                              |
|        /History        |                                    Muestra todos los comandos que se almacenan en memoria. Puede utilizar el símbolo de redirección (**>**) con **/History** para redirigir la lista a un archivo. Se puede abreviar **/History** como **/h**.                                    |
| /insert | Especifica que el nuevo texto que escriba se insertará en el texto anterior. |
| /overstrike | Especifica que el nuevo texto sobrescribe el texto anterior. |
|  /EXENAME =\<EXENAME>   |                                                                                        Especifica el programa (es decir, ejecutable) en el que se ejecuta la macro **doskey** .                                                                                         |
| /MACROFILE =\<nombrearchivo> |                                                                                              Especifica un archivo que contiene las macros que desea instalar.                                                                                               |
| \<Nombremacro>= [\<texto>]  | Crea una macro que lleva a cabo los comandos especificados por el *texto*. *Nombremacro* especifica el nombre que desea asignar a la macro. *Texto* especifica los comandos que desea registrar. Si el *texto* se deja en blanco, *nombremacro* se borra de los comandos asignados. |
|           /?           |                                                                                                              Muestra la ayuda en el símbolo del sistema.                                                                                                              |

## <a name="remarks"></a>Observaciones

- Usar Doskey. exe

  Doskey. exe siempre está disponible para todos los programas interactivos basados en caracteres (como depuradores de programas o programas de transferencia de archivos) y mantiene un búfer de historial de comandos y macros para cada programa que se inicia. No puede utilizar las opciones de línea de comandos de **doskey** desde un programa. Debe ejecutar las opciones de línea de comandos de **doskey** antes de iniciar un programa. Las asignaciones de clave de programa invalidan las asignaciones de teclas de **doskey** .
- Recuperando un comando

  Para recuperar un comando, puede utilizar cualquiera de las siguientes claves después de iniciar Doskey. exe. Si utiliza Doskey. exe dentro de un programa, las asignaciones de clave de ese programa tienen prioridad.  

  |    Clave     |                              Descripción                              |
  |------------|-----------------------------------------------------------------------|
  |  FLECHA ARRIBA  |  Recupera el comando que usó antes del que se muestra.  |
  | FLECHA ABAJO |  Recupera el comando que usó después del que se muestra.   |
  |  RE PÁG   |    Recupera el primer comando que usó en la sesión actual.    |
  | AV PÁG  | Recupera el comando más reciente que usó en la sesión actual. |


- Editar la línea de comandos

  Con Doskey. exe, puede editar la línea de comandos actual. Si utiliza Doskey. exe dentro de un programa, las asignaciones de clave de ese programa tienen prioridad y es posible que algunas claves de edición de Doskey. exe no funcionen.

  En la tabla siguiente se enumeran las claves de edición de **doskey** y sus funciones.  

  | Tecla o combinación de teclas |                                                                                                                                                       Descripción                                                                                                                                                       |
  |------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |       FLECHA IZQUIERDA       |                                                                                                                                      Mueve el punto de inserción un carácter hacia atrás.                                                                                                                                      |
  |      FLECHA DERECHA       |                                                                                                                                    Mueve el punto de inserción hacia delante un carácter.                                                                                                                                     |
  |    CTRL+FLECHA IZQUIERDA     |                                                                                                                                        Mueve el punto de inserción una palabra hacia atrás.                                                                                                                                         |
  |    CTRL+FLECHA DERECHA    |                                                                                                                                       Mueve el punto de inserción hacia delante una palabra.                                                                                                                                       |
  |          INICIO          |                                                                                                                                 Mueve el punto de inserción al principio de la línea.                                                                                                                                 |
  |          END           |                                                                                                                                    Mueve el punto de inserción al final de la línea.                                                                                                                                    |
  |          ESC           |                                                                                                                                          Borra el comando de la pantalla.                                                                                                                                           |
  |           F1           |                                                                      Copia un carácter de una columna de la plantilla en la misma columna de la ventana del símbolo del sistema. (La plantilla es un búfer de memoria que contiene el último comando que escribió).                                                                       |
  |           F2           |                                                                 Busca hacia delante en la plantilla la siguiente tecla que escriba después de presionar F2. Doskey. exe inserta el texto de la plantilla, hasta el carácter especificado, pero sin incluirlo.                                                                  |
  |           F3           |                                                 Copia el resto de la plantilla en la línea de comandos. Doskey. exe comienza a copiar caracteres desde la posición de la plantilla que corresponde a la posición indicada por el punto de inserción en la línea de comandos.                                                 |
  |           F4           |                                                                            Elimina todos los caracteres de la posición actual del punto de inserción hasta la siguiente aparición del carácter que se escribe después de presionar F4, sin incluirlo.                                                                            |
  |           F5           |                                                                                                                                   Copia la plantilla en la línea de comandos actual.                                                                                                                                    |
  |           F6           |                                                                                                                    Coloca un carácter de fin de archivo (CTRL + Z) en la posición del punto de inserción actual.                                                                                                                    |
  |           F7           | Muestra (en un cuadro de diálogo) todos los comandos para este programa que se almacenan en memoria. Use la tecla flecha arriba y la tecla flecha abajo para seleccionar el comando que desee y presione Entrar para ejecutar el comando. También puede anotar el número secuencial delante del comando y usar este número junto con la tecla F9. |
  |         ALT+F7         |                                                                                                                          Elimina todos los comandos almacenados en memoria para el búfer de historial actual.                                                                                                                          |
  |           F8           |                                                                                                           Muestra todos los comandos del búfer de historial que comienzan con los caracteres del comando actual.                                                                                                            |
  |           F9           |                                             Solicita un número de comando de búfer de historial y, a continuación, muestra el comando asociado al número que especifique. Presione Entrar para ejecutar el comando. Para mostrar todos los números y sus comandos asociados, presione F7.                                             |
  |        ALT+F10         |                                                                                                                                             Elimina todas las definiciones de macro.                                                                                                                                              |


- Usar **doskey** dentro de un programa

  Ciertos programas interactivos basados en caracteres, como depuradores de programas o programas de transferencia de archivos (FTP), utilizan automáticamente Doskey. exe. Para usar Doskey. exe, un programa debe ser un proceso de consola y usar una entrada almacenada en búfer. Las asignaciones de clave de programa invalidan las asignaciones de teclas de **doskey** . Por ejemplo, si el programa utiliza la tecla F7 para una función, no se puede obtener un historial de comandos de **doskey** en una ventana emergente.

  Con Doskey. exe, puede mantener un historial de comandos para cada programa que inicie o repita. Puede editar los comandos anteriores en el símbolo del sistema e iniciar las macros de **doskey** creadas para el programa. Si sale y después reinicia un programa desde la misma ventana del símbolo del sistema, está disponible el historial de comandos de la sesión de programa anterior.

  Debe ejecutar Doskey. exe antes de iniciar un programa. No puede utilizar las opciones de línea de comandos de **doskey** desde el símbolo del sistema de un programa, aunque el programa tenga un comando de Shell.

  Si desea personalizar el funcionamiento de Doskey. exe con un programa y crear macros de **doskey** para ese programa, puede crear un programa por lotes que modifique Doskey. exe e inicie el programa.
- Especificar un modo de inserción predeterminado

  Si presiona la tecla insertar, puede escribir texto en la línea de comandos de **doskey** en medio de texto existente sin reemplazar el texto. Sin embargo, después de presionar entrar, Doskey. exe devuelve el teclado para reemplazar el modo. Debe presionar insertar de nuevo para volver al modo de inserción.

  Use **/Insert** para cambiar el teclado al modo de inserción cada vez que presione Entrar. El teclado permanece en modo de inserción hasta que use **/OVERSTRIKE**. Puede volver temporalmente al modo reemplazar presionando la tecla insertar, pero después de presionar entrar, Doskey. exe devuelve el teclado al modo insertar.

  El punto de inserción cambia de forma cuando se usa la tecla insertar para cambiar de un modo a otro.
- Crear una macro

  Puede usar Doskey. exe para crear macros que realicen uno o más comandos. En la tabla siguiente se enumeran los caracteres especiales que se pueden usar para controlar las operaciones de comandos cuando se define una macro.  

  |   Carácter   |                                                                                                                                                                               Descripción                                                                                                                                                                               |
  |---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |   $G o $g    |                                                                                   Redirige la salida. Use cualquiera de estos caracteres especiales para enviar la salida a un dispositivo o un archivo en lugar de a la pantalla. Este carácter es equivalente al símbolo de redirección de Output (**>**).                                                                                    |
  | $G $ G o $g $ g  |                                                         Anexa la salida al final de un archivo. Use cualquiera de estos dos caracteres dobles para anexar la salida a un archivo existente en lugar de reemplazar los datos del archivo. Estos caracteres dobles son equivalentes al símbolo de redirección de anexado**>>** para la salida ().                                                         |
  |   $L o $l    |                                                                                  Redirige la entrada. Use cualquiera de estos caracteres especiales para leer la entrada de un dispositivo o un archivo en lugar de hacerlo desde el teclado. Este carácter es equivalente al símbolo de redirección para la entrada**<**().                                                                                  |
  |   $B o $b    |                                                                                                                                    Envía la salida de la macro a un comando. Estos caracteres especiales son equivalentes al uso de la canalización (\*\*                                                                                                                                     |
  |   $T o $t    |                                                            Separa los comandos. Use cualquiera de estos caracteres especiales para separar comandos cuando cree macros o comandos de tipo en la línea de comandos de **doskey** . Estos caracteres especiales son equivalentes al uso del símbolo**&** de y comercial () en una línea de comandos.                                                            |
  |      $$       |                                                                                                                                                              Especifica el carácter de signo de dólar**$**().                                                                                                                                                               |
  | de $1 a $9 |             Represente la información de línea de comandos que desee especificar al ejecutar la macro. Los caracteres especiales de **$1** a **$9** son parámetros de lote que permiten usar datos diferentes en la línea de comandos cada vez que se ejecuta la macro. El carácter **$1** de un comando **doskey** es similar al carácter **%1** en un programa por lotes.             |
  |      $\*      | Representa toda la información de línea de comandos que desea especificar al escribir el nombre de la macro. El ** $ ** carácter \* especial es un parámetro reemplazable que es similar a los parámetros del lote de **$1** a **$9**, con una diferencia importante: todo lo que escriba en la línea de comandos después de que el nombre de ** $ ** la macro se sustituya por el \* de la macro. |


- Ejecutar una macro **doskey**

  Para ejecutar una macro, escriba el nombre de la macro en el símbolo del sistema, comenzando en la primera posición. Si la macro se definió ** $ **con * o con cualquiera de los parámetros del lote de **$1** a **$9**, use un espacio para separar los parámetros. No se puede ejecutar una macro **doskey** desde un programa por lotes.
- Crear una macro con el mismo nombre que un comando de la familia de Windows Server 2003

  Si siempre usa un comando determinado con opciones específicas de la línea de comandos, puede crear una macro que tenga el mismo nombre que el comando. Para especificar si desea ejecutar la macro o el comando, siga estas instrucciones:  
  -   Para ejecutar la macro, escriba el nombre de la macro en el símbolo del sistema. No agregue un espacio delante del nombre de la macro.
  -   Para ejecutar el comando, inserte uno o más espacios en el símbolo del sistema y, a continuación, escriba el nombre del comando.
- Eliminar una macro

  Para eliminar una macro, escriba:  
  ```
  doskey <MacroName> =
  ```

## <a name="examples"></a>Ejemplos

Las opciones de línea de comandos **/macros** y **/History** son útiles para crear programas de batch para guardar macros y comandos. Por ejemplo, para almacenar todas las macros de **doskey** actuales, escriba:
```
doskey /macros > macinit 
```
Para usar las macros almacenadas en Macinit, escriba:
```
doskey /macrofile=macinit 
```
Para crear un programa de Batch denominado tmp. bat que contenga los comandos usados recientemente, escriba:
```
doskey /history> tmp.bat 
```
Para definir una macro con varios comandos, use **$t** para separar comandos, como se indica a continuación:
```
doskey tx=cd temp$tdir/w $*
```
En el ejemplo anterior, la macro TX cambia el directorio actual a Temp y, a continuación, muestra una lista de directorios en formato de visualización ancho. Puede usar ** $ *** al final de la macro para anexar otras opciones de línea de comandos a **dir** al ejecutar TX.

La siguiente macro usa un parámetro batch para un nuevo nombre de directorio:
```
doskey mc=md $1$tcd $1
```
La macro crea un nuevo directorio y, a continuación, cambia al directorio nuevo desde el directorio actual.

Para usar la macro anterior para crear y cambiar a un directorio denominado Books, escriba:
```
mc books
```
Para crear una macro **doskey** para un programa denominado FTP. exe, incluya **/EXENAME** como se indica a continuación:
```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye 
```
Para usar la macro anterior, inicie FTP. En el símbolo del sistema FTP, escriba:
```
go
```
FTP ejecuta los comandos **Open**, **mget**y **bye** .

Para crear una macro que dé formato a un disco de forma rápida e incondicional, escriba:
```
doskey qf=format $1 /q /u
```
Para formatear rápida y incondicionalmente un disco en la unidad A, escriba:
```
qf a:
```
Para eliminar una macro denominada Vlist, escriba:
```
doskey vlist =
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
