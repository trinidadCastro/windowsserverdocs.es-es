---
title: xcopy
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 885729f2bca100d7ac89a3463135d56f48c8b75a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361792"
---
# <a name="xcopy"></a>xcopy

Copia archivos y directorios, incluidos los subdirectorios.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Source >|Obligatorio. Especifica la ubicación y los nombres de los archivos que desea copiar. Este parámetro debe incluir una unidad o una ruta de acceso.|
|[\<Destination >]|Especifica el destino de los archivos que desea copiar. Este parámetro puede incluir una letra de unidad y dos puntos, un nombre de directorio, un nombre de archivo o una combinación de estos.|
|/w|Muestra el siguiente mensaje y espera la respuesta antes de empezar a copiar los archivos:</br>**Presione cualquier tecla para empezar a copiar los archivos.**|
|/p|Le pide que confirme si desea crear cada archivo de destino.|
|/c|Omite los errores.|
|/v|Comprueba cada archivo a medida que se escribe en el archivo de destino para asegurarse de que los archivos de destino son idénticos a los archivos de origen.|
|/q|Suprime la presentación de mensajes **xcopy** .|
|/f|Muestra los nombres de archivo de origen y de destino durante la copia.|
|/l|Muestra una lista de los archivos que se van a copiar.|
|/g|Crea archivos de *destino* descifrados cuando el destino no admite el cifrado.|
|/d [: MM-DD-YYYY]|Copia los archivos de código fuente que han cambiado en o después de la fecha especificada. Si no incluye un valor *MM-DD-YYYY* , **xcopy** copia todos los archivos de *origen* que son más recientes que los archivos de *destino* existentes. Esta opción de línea de comandos permite actualizar los archivos que han cambiado.|
|/u|Copia archivos del *origen* que solo existen en el *destino* .|
|/i|Si el *origen* es un directorio o contiene caracteres comodín y el *destino* no existe, **xcopy** supone que el *destino* especifica un nombre de directorio y crea un nuevo directorio. A continuación, **xcopy** copia todos los archivos especificados en el nuevo directorio. De forma predeterminada, **xcopy** le pide que especifique si el *destino* es un archivo o un directorio.|
|/s|Copia directorios y subdirectorios, a menos que estén vacíos. Si omite **/s**, **xcopy** funciona dentro de un único directorio.|
|/e|Copia todos los subdirectorios, incluso si están vacíos. Use **/e** con las opciones de línea de comandos **/s** y **/t** .|
|/t|Copia la estructura de subdirectorio (es decir, el árbol) únicamente, no los archivos. Para copiar directorios vacíos, debe incluir la opción de línea de comandos **/e** .|
|/k|Copia los archivos y conserva el atributo de solo lectura en los archivos de *destino* , si está presente en los archivos de *código fuente* . De forma predeterminada, **xcopy** quita el atributo de solo lectura.|
|/r|Copia archivos de solo lectura.|
|/h|Copia archivos con atributos ocultos y de archivo del sistema. De forma predeterminada, **xcopy** no copia los archivos ocultos o del sistema|
|/a|Copia solo los archivos de *origen* que tienen establecidos los atributos de archivo de almacenamiento. **/a** no modifica el atributo de archivo de almacenamiento del archivo de origen. Para obtener información sobre cómo establecer el atributo de archivo de almacenamiento mediante **attrib**, consulte [referencias adicionales](#additional-references).|
|/m|Copia los archivos de *origen* que tienen los atributos de archivo de almacenamiento establecidos. A diferencia de **/a**, **/m** desactiva los atributos de archivo de almacenamiento en los archivos que se especifican en el origen. Para obtener información sobre cómo establecer el atributo de archivo de almacenamiento mediante **attrib**, consulte [referencias adicionales](#additional-references).|
|/n|Crea copias usando los nombres de archivo o directorio cortos NTFS. **/n** es necesario cuando se copian archivos o directorios de un volumen NTFS a un volumen FAT o cuando se requiere la Convención de nomenclatura del sistema de archivos FAT (es decir, 8,3 caracteres) en el sistema de archivos de *destino* . El sistema de archivos de *destino* puede ser FAT o NTFS.|
|/o|Copia la propiedad del archivo y la información de la lista de control de acceso discrecional (DACL).|
|/x|Copia la configuración de auditoría de archivos y la información de la lista de control de acceso (SACL) del sistema (implica **/o**).|
|/exclude: nombreDeArchivo1 [+ [Nombredearchivo2] [+ [FileName3] (\)]|Especifica una lista de archivos. Se debe especificar al menos un archivo. Cada archivo contendrá cadenas de búsqueda con cada cadena en una línea independiente del archivo.</br>Cuando cualquiera de las cadenas coincida con cualquier parte de la ruta de acceso absoluta del archivo que se va a copiar, el archivo se excluirá de la copia. Por ejemplo, si se especifica la cadena **obj** , se excluirán todos los archivos situados debajo del archivo **obj** o todos los archivos con la extensión **. obj** .|
|/y|Suprime el mensaje para confirmar que desea sobrescribir un archivo de destino existente.|
|/-y|Solicita que confirme si desea sobrescribir un archivo de destino existente.|
|/z|Copia a través de una red en modo reiniciable.|
|b|Copia el vínculo simbólico en lugar de los archivos. Este parámetro se incorporó en® de Windows Vista.|
|/j|Copia archivos sin almacenamiento en búfer. Recomendado para archivos de gran tamaño. Este parámetro se agregó en Windows Server 2008 R2.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Usar **/z**

  Si pierde la conexión durante la fase de copia (por ejemplo, si el servidor se desconecta de la conexión), se reanudará después de volver a establecer la conexión. **/z** también muestra el porcentaje de la operación de copia completada para cada archivo.

- Usar **/y** en la variable de entorno COPYCMD.

  Puede usar **/y** en la variable de entorno COPYCMD. Puede invalidar este comando mediante el uso de **/-y** en la línea de comandos. De forma predeterminada, se le pedirá que sobrescriba.

- Copiar archivos cifrados

  La copia de archivos cifrados en un volumen que no admita EFS produce un error. Descifre primero los archivos o copie los archivos en un volumen que admita EFS.

- Anexar archivos

  Para anexar archivos, especifique un único archivo para el destino, pero varios archivos para el origen (es decir, mediante caracteres comodín o el formato archivo1 + archivo2 + archivo3).

- Valor predeterminado de *destino*

  Si omite *Destination*, el comando **xcopy** copia los archivos en el directorio actual.

- Especificar si el *destino* es un archivo o un directorio

  Si el *destino* no contiene un directorio existente y no termina con una barra diagonal inversa (\), aparece el siguiente mensaje:
  
  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```  
  
Presione F si desea que el archivo o los archivos se copien en un archivo. Presione D si desea que el archivo o los archivos se copien en un directorio.

  Puede suprimir este mensaje mediante la opción de línea de comandos **/i** , que hace que **xcopy** asuma que el destino es un directorio si el origen es más de un archivo o un directorio.
- Usar el comando **xcopy** para establecer el atributo de archivo para los archivos de *destino*

  El comando **xcopy** crea archivos con el atributo Archive establecido, tanto si este atributo se estableció como si no en el archivo de código fuente. Para obtener más información sobre los atributos de archivo y el **atributo attrib**, consulte [referencias adicionales](#additional-references).

- Comparar **xcopy** y **diskcopy**

  Si tiene un disco que contiene archivos en subdirectorios y desea copiarlo en un disco con un formato diferente, utilice el comando **xcopy** en lugar de **diskcopy**. Dado que el comando **diskcopy** copia la pista de discos por pista, los discos de origen y de destino deben tener el mismo formato. El comando **xcopy** no tiene este requisito. Use **xcopy** a menos que necesite una copia de la imagen de disco completa.

- Códigos de salida de **xcopy**

  Para procesar los códigos de salida devueltos por **xcopy**, use el parámetro **ERRORLEVEL** en la línea de comandos **If** en un programa por lotes. Para ver un ejemplo de un programa por lotes que procesa los códigos de salida mediante **If**, consulte [referencias adicionales](#additional-references). En la tabla siguiente se enumeran los códigos de salida y una descripción.  

  |Código de salida|Descripción|
  |---------|-----------|
  |0|Los archivos se copiaron sin errores.|
  |1|No se encontraron archivos para copiar.|
  |2|El usuario presionó CTRL + C para finalizar **xcopy**.|
  |4|Error de inicialización. No hay suficiente memoria o espacio en disco, o bien se especificó un nombre de unidad no válido o una sintaxis no válida en la línea de comandos.|
  |5|Error al escribir en el disco.|

## <a name="examples"></a>Ejemplos

**1.** Para copiar todos los archivos y subdirectorios (incluidos los subdirectorios vacíos) de la unidad a a la unidad B, escriba:

```
xcopy a: b: /s /e 
```

**2.** Para incluir cualquier archivo del sistema o oculto en el ejemplo anterior, agregue la opción de línea de comandos<strong>/h</strong> de la siguiente manera:

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

**5.** Para obtener una lista de los archivos que va a copiar el comando anterior (es decir, sin copiar realmente los archivos), escriba:

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

El archivo XCopy. out muestra todos los archivos que se van a copiar.

**1,8.** Para copiar el directorio \Customer y todos los subdirectorios en el directorio \\ @ no__t-1Public\Address en la unidad de red H:, conserve el atributo de solo lectura y se le preguntará cuando se cree un nuevo archivo en H:, escriba:

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** Para emitir el comando anterior, asegúrese de que **xcopy** cree el directorio \Address si no existe y suprima el mensaje que aparece al crear un directorio nuevo, agregue la opción de línea de comandos **/i** de la siguiente manera:

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**203.** Puede crear un programa por lotes para realizar operaciones **xcopy** y usar el comando batch **If** para procesar el código de salida si se produce un error. Por ejemplo, el siguiente programa por lotes usa parámetros reemplazables para los parámetros de origen y destino de **xcopy** :

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

Para usar el programa por lotes anterior para copiar todos los archivos del directorio C:\Prgmcode y sus subdirectorios en la unidad B, escriba:

```
copyit c:\prgmcode b:
```

El intérprete de comandos sustituye a **C:\Prgmcode** para *% 1* y **B:** para *% 2*, utiliza **xcopy** con las opciones de línea de comandos **/e** y **/s** . Si **xcopy** detecta un error, el programa por lotes lee el código de salida y dirige a la etiqueta indicada en la instrucción **If ERRORLEVEL** adecuada y, a continuación, muestra el mensaje correspondiente y sale del programa por lotes.

**9.** En este ejemplo, todos los directorios no vacíos, además de los archivos cuyo nombre coincide con el patrón dado con el símbolo de asterisco.

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

En el ejemplo anterior, este valor de parámetro de origen concreto **. @no__t -1toc\*.yml** copie los mismos 3 archivos aunque se hayan quitado sus dos caracteres de ruta de acceso **. \\** . Sin embargo, no se copiará ningún archivo si se quitó el carácter comodín de asterisco del parámetro de origen, lo que lo hizo simplemente **. @no__t -1toc. yml**.

#### <a name="additional-references"></a>Referencias adicionales

-   [Copiar](copy.md)
-   [Mover](move.md)
-   [Dir](dir.md)
-   [Atributo](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [Cuando](if.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
