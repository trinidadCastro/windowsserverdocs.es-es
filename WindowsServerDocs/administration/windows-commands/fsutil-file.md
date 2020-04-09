---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: Fsutil (archivo)
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 175b5e17f186653d4fdbc7efb505637e915cfe38
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844328"
---
# <a name="fsutil-file"></a>Fsutil (archivo)
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Busca un archivo por su nombre de usuario (si las cuotas de disco están habilitadas), consulta los intervalos asignados de un archivo, establece el nombre corto de un archivo, establece la longitud de datos válidos de un archivo, establece cero datos para un archivo o crea un archivo nuevo.

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

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|CreateNew|Crea un archivo con el nombre y el tamaño especificados, con contenido que consta de ceros.|
|\<nombre de archivo >|Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo C:\documents\filename.txt.|
|longitud de \<>|Especifica la longitud de datos válidos del archivo.|
|findbysid|Busca los archivos que pertenecen a un usuario especificado en volúmenes NTFS en los que se habilitan las cuotas de disco.|
|\<nombre de usuario >|Especifica el nombre de usuario o el nombre de inicio de sesión del usuario.|
|> de \<de directorio|Especifica la ruta de acceso completa al directorio, por ejemplo C:\Users.|
|optimizemetadata|Esto realiza una compactación inmediata de los metadatos de un archivo determinado.|
|/A|Analizar metadatos de archivo antes y después de la optimización.|
|queryallocranges|Consulta los intervalos asignados de un archivo en un volumen NTFS. Resulta útil para determinar si un archivo tiene regiones dispersas.|
|offset = desplazamiento de\<>|Especifica el inicio del intervalo que se debe establecer en ceros.|
|longitud =\<longitud >|Especifica la longitud del intervalo (en bytes).|
|queryextents|Consulta extensiones para un archivo.|
|/R|Si <filename> es un punto de repetición de análisis, ábralo en lugar de su destino.|
|\<startingvcn >|Especifica el primer VCN que se va a consultar. Si se omite, empiece en VCN 0.|
|\<numvcns >|Número de VCNs que se van a consultar. Si se omite o es 0, consulta hasta EOF.|
|queryfileid|Consulta el ID. de archivo de un archivo en un volumen NTFS.<p>Este parámetro se aplica a: Windows Server 2008 R2 y Windows 7.|
|\<> de volumen|Especifica el volumen como el nombre de la unidad seguido de dos puntos.|
|queryfilenamebyid|Muestra un nombre de vínculo aleatorio para un identificador de archivo especificado en un volumen NTFS. Dado que un archivo puede tener más de un nombre de vínculo que apunta a ese archivo, no se garantiza que el vínculo de archivo se proporcione como resultado de la consulta para el nombre de archivo.<p>Este parámetro se aplica a: Windows Server 2008 R2 y Windows 7.|
|\<fileid >|Especifica el identificador del archivo en un volumen NTFS.|
|queryoptimizemetadata|Consulta el estado de los metadatos de un archivo.|
|queryvaliddata|Consulta la longitud de datos válida para un archivo.|
|/D|Muestra información detallada de los datos válidos.|
|seteo|Establece el EOF del archivo especificado.|
|setshortname|Establece el nombre corto (8,3 nombre de archivo de longitud de caracteres) para un archivo en un volumen NTFS.|
|\<nombre_corto >|Especifica el nombre corto del archivo.|
|setvaliddata|Establece la longitud de datos válida para un archivo en un volumen NTFS.|
|\<DATALENGTH >|Especifica la longitud del archivo en bytes.|
|setzerodata|Establece un intervalo (especificado por el *desplazamiento* y la *longitud*) del archivo en ceros, que vacía el archivo. Si el archivo es un archivo disperso, las unidades de asignación subyacentes se desasignan.|

## <a name="remarks"></a>Comentarios

-   En NTFS, hay dos conceptos importantes de longitud de archivo: el marcador de fin de archivo (EOF) y la longitud de datos válida (VDL). EOF indica la longitud real del archivo. VDL identifica la longitud de los datos válidos en el disco. Las lecturas entre VDL y EOF devuelven automáticamente 0 para conservar el requisito de reutilización del objeto C2.

-   El parámetro **setvaliddata** solo está disponible para los administradores porque requiere el privilegio realizar tareas de mantenimiento del volumen (SeManageVolumePrivilege). Esta característica solo es necesaria para escenarios avanzados de redes de área de sistema y multimedia. El parámetro **setvaliddata** debe ser un valor positivo mayor que el VDL actual, pero menor que el tamaño actual del archivo.

    Es útil para los programas establecer un VDL cuando:

    -   Escritura de clústeres sin procesar directamente en el disco a través de un canal de hardware. Esto permite al programa informar al sistema de archivos de que este intervalo contiene datos válidos que se pueden devolver al usuario.

    -   Crear archivos grandes cuando el rendimiento es un problema. Esto evita el tiempo que se tarda en rellenar el archivo con ceros cuando se crea o se extiende el archivo.

## <a name="examples"></a><a name="BKMK_examples"></a>Example
Para buscar archivos que son propiedad de scottb en la unidad C, escriba:

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

Para consultar las extensiones de un archivo, escriba:

```
fsutil file queryextents C:\Temp\sample.txt
```

Para establecer EOF para un archivo, escriba:

```
fsutil file seteof C:\testfile.txt 1000
```

Para establecer el nombre corto del archivo Longfilename. txt de la unidad C en Longfile. txt, escriba:

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

Para establecer la longitud de datos válida en 4096 bytes para un archivo denominado TESTFILE. txt en un volumen NTFS, escriba:

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

Para establecer un intervalo de un archivo en un volumen NTFS en ceros para vaciarlo, escriba:

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


