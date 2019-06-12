---
title: xcopy
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: 6448c4c5940d286931f6d64ad51970bf577a28fd
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811025"
---
# <a name="xcopy"></a>xcopy

Copia los archivos y directorios, incluidos los subdirectorios.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<origen >|Obligatorio. Especifica la ubicación y los nombres de los archivos que van a copiar. Este parámetro debe incluir una unidad o una ruta de acceso.|
|[\<Destino >]|Especifica el destino de los archivos que van a copiar. Este parámetro puede incluir una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos.|
|/w|Muestra el siguiente mensaje y espera la respuesta antes de empezar a copiar los archivos:</br>**Presione cualquier tecla para empezar a copiar los archivos**|
|/p|Le pide que confirme si desea crear cada archivo de destino.|
|/c|Omite los errores.|
|/v|Comprueba cada archivo tal como se escribe en el archivo de destino para asegurarse de que los archivos de destino son idénticos a los archivos de origen.|
|/q|Suprime la presentación de **xcopy** mensajes.|
|/f|Muestra los nombres de archivo de origen y de destino mientras se copian.|
|/l|Muestra una lista de archivos que se va a copiar.|
|/g|Crea descifrada *destino* cuando el destino no admite el cifrado de archivos.|
|/d [: MM-DD-AAAA]|Copia los archivos modificados en o después de la fecha especificada sólo de origen. Si no incluye un *MM-DD-AAAA* valor, **xcopy** todos los copia *origen* archivos que son más recientes que existente *destino* archivos. Esta opción de línea de comandos permite actualizar los archivos que han cambiado.|
|/u|Copia los archivos de *origen* que existen en *destino* solo.|
|/i|Si *origen* es un directorio o contiene caracteres comodín y *destino* no existe, **xcopy** supone *destino* especifica un nombre de directorio y crea un nuevo directorio. A continuación, **xcopy** copia todos los archivos especificados en el directorio nuevo. De forma predeterminada, **xcopy** le pide que especifique si *destino* es un archivo o un directorio.|
|/s|Copia los directorios y subdirectorios, a menos que estén vacíos. Si se omite **/s**, **xcopy** funciona dentro de un único directorio.|
|/e|Copia todos los subdirectorios, incluso si están vacías. Use **/e** con el **/s** y **/t** opciones de línea de comandos.|
|/t|Copia la estructura de subdirectorio (es decir, el árbol), no los archivos. Para copiar directorios vacíos, debe incluir el **/e** opción de línea de comandos.|
|/k|Copia los archivos y conserva el atributo de solo lectura en *destino* archivos si están presentes en el *origen* archivos. De forma predeterminada, **xcopy** quita el atributo de sólo lectura.|
|/r|Copia los archivos de solo lectura.|
|/h|Copia los archivos con ocultos y los atributos de archivo del sistema. De forma predeterminada, **xcopy** no copia ocultados ni archivos del sistema|
|/a|Copia únicamente *origen* archivos que tienen su archivo de conjunto de atributos. **/a** no modifica el atributo de archivo del archivo del archivo de origen. Para obtener información acerca de cómo establecer el atributo de archivo modificado mediante **attrib**, consulte [referencias adicionales](#additional-references).|
|/m|Copias *origen* archivos que tienen su archivo de conjunto de atributos. A diferencia de **/a**, **/m** desactiva los atributos de archivo del archivo en los archivos que se especifican en el origen. Para obtener información acerca de cómo establecer el atributo de archivo modificado mediante **attrib**, consulte [referencias adicionales](#additional-references).|
|/n|Crea copias mediante cortos de archivo NTFS o nombres de directorio. **/n** es necesario cuando copie archivos o directorios de un volumen NTFS a un volumen FAT o cuando el consumo de grasa convención de nomenclatura del sistema (es decir, con formato 8.3 caracteres) del archivo es necesario en el *destino* sistema de archivos. El *destino* puede ser el sistema de archivos FAT o NTFS.|
|/o|Copia archivos de propiedad y la información de lista (DACL) del control de acceso discrecional.|
|/x|Configuración de auditoría y la información de lista (SACL) de control de acceso del sistema de archivos de copias (implica **/o**).|
|/exclude:FileName1[+[FileName2][+[FileName3]( \)]|Especifica una lista de archivos. Debe especificarse al menos un archivo. Cada archivo contendrá las cadenas de búsqueda con cada cadena en una línea independiente en el archivo.</br>Cuando cualquiera de las cadenas coinciden con cualquier parte de la ruta de acceso absoluta del archivo que se va a copiar, ese archivo se excluirán de la copia. Por ejemplo, si se especifica la cadena **obj** excluirá todos los archivos del directorio **obj** o todos los archivos con la **.obj** extensión.|
|/y|Suprime el mensaje para confirmar que desea sobrescribir un archivo de destino existente.|
|/-y|Le pide que confirme que desea sobrescribir un archivo de destino existente.|
|/z|Copia a través de una red en modo reiniciable.|
|/b|Copia el vínculo simbólico en lugar de los archivos. Este parámetro se incorporó en Windows Vista®.|
|/j|Copia los archivos sin almacenar en búfer. Se recomienda para los archivos muy grandes. Este parámetro se incorporó en Windows Server 2008 R2.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Uso de   **/z**

  Si pierde la conexión durante la fase de copia (por ejemplo, si el servidor está quedándose sin conexión interrumpe la conexión), se reanuda después de restablecer la conexión. **/ z** también muestra el porcentaje de la operación de copia que se ha completado en cada archivo.

- Uso de **/y** en apunta el vínculo.

  Puede usar **/y** en apunta el vínculo. Este comando se puede reemplazar utilizando **/y** en la línea de comandos. De forma predeterminada, se le pida para sobrescribir.

- Copiar archivos cifrados

  Copiar archivos cifrados a un volumen que no es compatible con EFS se produce un error. Descifrar en primer lugar los archivos o copie los archivos en un volumen que es compatible con EFS.

- Anexar a archivos

  Para anexar archivos, especifique un único archivo de destino, pero varios archivos de origen (es decir, con caracteres comodín o file1 + file2 + file3 formato).

- Valor predeterminado para *destino*

  Si se omite *destino*, **xcopy** comando copia los archivos en el directorio actual.

- Especifica si *destino* es un archivo o directorio

  Si *destino* no contiene un directorio existente y no termina con una barra diagonal inversa (\), aparece el mensaje siguiente:
  
  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```  
  
Si desea que los archivos se copien en un archivo, presione F. Presione D si desea que los archivos se copien en un directorio.

  Puede suprimir este mensaje con el **/i** la opción de línea de comandos, lo que hace que **xcopy** suponer que el destino es un directorio si el origen es más de un archivo o un directorio.
- Mediante el **xcopy** comando para establecer el atributo archive de *destino* archivos

  El **xcopy** comando crea archivos con el conjunto de atributos de archivo, si este atributo se estableció en el archivo de origen. Para obtener más información acerca de los atributos de archivo y **attrib**, consulte [referencias adicionales](#additional-references).

- Comparar **xcopy** y **diskcopy**

  Si tiene un disco que contiene los archivos en los subdirectorios y desea copiarlo en un disco que tiene un formato diferente, use el **xcopy** comando en lugar de **diskcopy**. Dado que el **diskcopy** comando copia los discos pista por pista, los discos de origen y destino deben tener el mismo formato. El **xcopy** comando no tiene este requisito. Use **xcopy** a menos que necesite una copia de la imagen de disco completo.

- Códigos de salida de **xcopy**

  Para procesar los códigos de salida devueltos por **xcopy**, utilice el **ErrorLevel** parámetro en el **si** línea de comandos en un programa por lotes. Para obtener un ejemplo de un programa por lotes que los procesos de códigos de salida con **si**, consulte [referencias adicionales](#additional-references). En la tabla siguiente se enumera los códigos de salida y una descripción.  

  |Código de salida|Descripción|
  |---------|-----------|
  |0|Se copiaron los archivos sin errores.|
  |1|No se encontraron archivos para copiar.|
  |2|El usuario presionó CTRL+C para finalizar **xcopy**.|
  |4|Se produjo el error de inicialización. No hay suficiente memoria o espacio en disco o que escribió un nombre de unidad no válida o una sintaxis no válida en la línea de comandos.|
  |5|Error de escritura de disco.|

## <a name="examples"></a>Ejemplos

**1.** Para copiar todos los archivos y subdirectorios (incluidos los subdirectorios vacíos) desde la unidad A B de la unidad, escriba:

```
xcopy a: b: /s /e 
```

**2.** Para incluir cualquier sistema o los archivos ocultos en el ejemplo anterior, agregue el<strong>/h</strong> la opción de línea de comandos como sigue:

```
xcopy a: b: /s /e /h
```

**3.** Para actualizar los archivos del directorio \Reports con los archivos del directorio \Rawdata que han cambiado desde el 29 de diciembre de 1993, escriba:

```
xcopy \rawdata \reports /d:12-29-1993
```

**4.** Para actualizar todos los archivos que existen en \Reports en el ejemplo anterior, independientemente de la fecha, escriba:

```
xcopy \rawdata \reports /u
```

**5.** Para obtener una lista de los archivos que se va a copiar mediante el comando anterior (es decir, sin tener que copiar los archivos), escriba:

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

El archivo de XCOPY.out contiene una muestra todos los archivos que se va a copiar.

**6.** Para copiar el directorio \Customer y todos los subdirectorios en el directorio \\ \\Public\Address en red unidad H:, conservar el atributo de solo lectura y se le solicite cuando se crea un nuevo archivo en H:, escriba:

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** Para emitir el comando anterior, asegúrese de que **xcopy** crea el directorio \Address si no existe y suprimir el mensaje que aparece cuando se crea un nuevo directorio, agregue el **/i** de línea de comandos la opción siguiente:

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**8.** Puede crear un programa por lotes para realizar **xcopy** operaciones y el uso del lote **si** comando para procesar el código de salida si se produce un error. Por ejemplo, el siguiente programa por lotes utiliza parámetros reemplazables para el **xcopy** parámetros de origen y destino:

```
@echo off
rem COPYIT.BAT transfers all files in all subdirectories of
rem the source drive or directory (%1) to the destination
rem drive or directory (%2)
xcopy %1 %2 /s /e
if errorlevel 4 goto lowmemory
if errorlevel 2 goto abort
if errorlevel 0 goto exit
:lowmemory
echo Insufficient memory to copy files or
echo invalid drive or command-line syntax.
goto exit
:abort
echo You pressed CTRL+C to end the copy operation.
goto exit
:exit 
```

Para usar el programa por lotes anterior para copiar todos los archivos en el directorio C:\Prgmcode y sus subdirectorios en la unidad B, escriba:

```
copyit c:\prgmcode b:
```

Los sustitutos de intérprete de comandos **C:\Prgmcode** para *%1* y **B:** para *%2*, a continuación, usa **xcopy**con el **/e** y **/s** opciones de línea de comandos. Si **xcopy** encuentra un error, el programa por lotes lee el código de salida y se dirige a la etiqueta indicada en los correspondientes **IF ERRORLEVEL** instrucción, a continuación, muestra el mensaje adecuado y sale de la programa por lotes.

**9.** En este ejemplo, todas el no vacía los directorios, además de los archivos cuyo nombre coincide con el patrón proporcionado con el símbolo de asterisco.

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

En el ejemplo anterior, este valor de parámetro de origen determinado **.\\ TDC\*.yml** copiar la misma 3 archivos incluso si los caracteres de ruta de dos acceso **.\\**  se han quitado. Sin embargo, no hay ningún archivo se copiarían si el carácter comodín de asterisco se quitó el parámetro de origen, lo que simplemente **.\\ TOC.yml**.

#### <a name="additional-references"></a>Referencias adicionales

-   [Copiar](copy.md)
-   [Mover](move.md)
-   [dir](dir.md)
-   [Attrib](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [If](if.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
