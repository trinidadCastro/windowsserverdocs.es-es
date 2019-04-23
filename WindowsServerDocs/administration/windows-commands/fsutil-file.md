---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: archivo de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ffaf02f74f20f4eb94b94d8f0ffc51f26a62390e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828126"
---
# <a name="fsutil-file"></a>archivo de fsutil
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Busca un archivo por el nombre de usuario (si están habilitadas las cuotas de disco), consulta los intervalos asignados para un archivo, Establece el nombre corto de un archivo, Establece la longitud de datos válidos de un archivo, Establece los datos de cero para un archivo o crea un nuevo archivo.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil file [createnew] <filename> <length>
fsutil file [findbysid] <username> <directory>
fsutil file [optimizemetadata] [/A] <filename>
fsutil file [queryallocranges] offset=<offset> length=<length> <filename>
fsutil file [queryextents] [/R] <filename> [<startingvcn> [<numvcns>]]
fsutil file [queryfileid] <filename>
fsutil file [queryfilenamebyid] <volume> <fileid>
fsutil file [queryoptimizemetadata] <filename>
fsutil file [queryvaliddata] [/R] [/D] <filename>
fsutil file [seteof] <filename> <length>
fsutil file [setshortname] <filename> <shortname>
fsutil file [setvaliddata] <filename> <datalength>
fsutil file [setzerodata] offset=<offset> length=<length> <filename>

```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|CreateNew|Crea un archivo del nombre especificado y el tamaño, con contenido formado por ceros.|
|\<filename>|Especifica la ruta de acceso completa al archivo incluido el nombre de archivo y la extensión, por ejemplo C:\documents\filename.txt.|
|\<length>|Especifica la longitud del archivo datos válidos.|
|findbysid|Busca archivos que pertenecen a un usuario especificado en volúmenes NTFS donde están habilitadas las cuotas de disco.|
|\<username>|Especifica el nombre de usuario o el nombre de inicio de sesión del usuario.|
|\<directory>|Especifica la ruta de acceso completa al directorio, por ejemplo C:\users.|
|optimizemetadata|Esto lleva a cabo una compactación inmediata de los metadatos de un archivo determinado.|
|/A|Analizar los metadatos del archivo antes y después de la optimización.|
|queryallocranges|Consulta los intervalos asignados de un archivo en un volumen NTFS. Resulta útil para determinar si un archivo tiene regiones dispersas.|
|offset=\<offset>|Especifica el inicio del intervalo que se debe establecer en ceros.|
|length=\<length>|Especifica la longitud del intervalo (en bytes).|
|queryextents|Extensiones de consultas para un archivo.|
|/R|Si <filename> es un reanálisis punto, abrir, en lugar de su destino.|
|\<startingvcn>|Especifica el primer VCN a la consulta. Si se omite, se iniciará en VCN 0.|
|\<numvcns>|Número de VCNs para consultar. Si se omite o es 0, consulta hasta EOF.|
|queryfileid|Consulta el Id. de archivo de un archivo en un volumen NTFS.<br /><br />Este parámetro se aplica a:  Windows Server 2008 R2 y Windows 7.|
|\<volume>|Especifica el volumen como nombre de la unidad seguido de dos puntos.|
|queryfilenamebyid|Muestra un nombre de vínculo aleatorio para un identificador de archivo especificado en un volumen NTFS. Puesto que un archivo puede tener más de un nombre de vínculo que apunta a ese archivo, no se garantiza qué archivo de vínculo se proporcionará como resultado de la consulta para el nombre de archivo.<br /><br />Este parámetro se aplica a:  Windows Server 2008 R2 y Windows 7.|
|\<fileid>|Especifica el identificador del archivo en un volumen NTFS.|
|queryoptimizemetadata|Consulta el estado de los metadatos de un archivo.|
|queryvaliddata|Consulta la longitud de datos válidos para un archivo.|
|/D|Mostrar información detallada de datos válido.|
|seteof|Establece el EOF del archivo dado.|
|setshortname|Establece el nombre corto (nombre del archivo de longitud de caracteres con formato 8.3) para un archivo en un volumen NTFS.|
|\<shortname>|Especifica el nombre corto del archivo.|
|setvaliddata|Establece la longitud de datos válidos para un archivo en un volumen NTFS.|
|\<datalength>|Especifica la longitud del archivo en bytes.|
|setzerodata|Establece un intervalo (especificado por *desplazamiento* y *longitud*) del archivo en ceros, que vacía el archivo. Si el archivo es un archivo disperso, las unidades de asignación subyacente se anula la confirmación.|

## <a name="remarks"></a>Comentarios

-   En NTFS, hay dos conceptos importantes de la longitud del archivo: el marcador de final de archivo (EOF) y la longitud de datos válidos (VDL). El EOF indica la longitud real del archivo. La VDL identifica la longitud de datos válidos en el disco. Las lecturas entre VDL y EOF automáticamente devuelven 0 para conservar el objeto C2 reutilizar el requisito.

-   El **setvaliddata** parámetro solo está disponible para los administradores porque requiere el privilegio de tareas (SeManageVolumePrivilege) realizar mantenimiento de volumen. Esta característica solo es necesario para escenarios avanzados de área de contenido multimedia y el sistema de red. El **setvaliddata** parámetro debe ser un valor positivo que es mayor que la VDL actual, pero menor que el tamaño del archivo actual.

    Resulta útil para los programas establecer una VDL cuando:

    -   Escritura de los clústeres sin procesar directamente en el disco a través de un canal de hardware. Esto permite que el programa informar al sistema de archivos que este intervalo contiene datos válidos que se pueden devolver al usuario.

    -   Creación de archivos de gran tamaño cuando el rendimiento es un problema. Esto evita que el tiempo necesario para rellenar el archivo con ceros cuando se crean o se extienden el archivo.

## <a name="BKMK_examples"></a>Ejemplos
Para buscar los archivos que pertenecen a scottb en la unidad C, escriba:

```
fsutil file findbysid scottb c:\users  
```

Para consultar los intervalos asignados de un archivo en un volumen NTFS, escriba:

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt  
```

Para optimizar los metadatos de un archivo, escriba:

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

Para consultar las extensiones para un archivo, escriba:

```
fsutil file queryextents C:\Temp\sample.txt
```

Para establecer el EOF para un archivo, escriba:

```
fsutil file seteof C:\testfile.txt 1000
```

Para establecer el nombre corto del archivo nombreDeArchivoLargo.txt en la unidad C para nomLargo.txt, escriba:

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

Para establecer la longitud de datos válida a 4096 bytes para un archivo denominado Testfile.txt en un volumen NTFS, escriba:

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

Para establecer un intervalo de un archivo en un volumen NTFS a los ceros a la vacía, escriba:

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


