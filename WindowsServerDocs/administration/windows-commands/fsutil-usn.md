---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: fsutil usn
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4bef63f4938b5ce786bbbfdbf3bdd2081280ce95
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829986"
---
# <a name="fsutil-usn"></a>fsutil usn
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra el diario de cambios actualización (USN) del número de secuencia.

## <a name="syntax"></a>Sintaxis

```
fsutil usn [createjournal] m=<MaxSize> a=<AllocationDelta> <VolumePath>
fsutil usn [deletejournal] {/D | /N} <volumepath>
fsutil usn [enablerangetracking] <volumepath> [options]
fsutil usn [enumdata] <FileRef> <LowUSN> <HighUSN> <VolumePath>
fsutil usn [queryjournal] <VolumePath>
fsutil usn [readdata] <FileName>
fsutil usn [readjournal] [c= <chunk-size> s=<file-size-threshold>] <volumepath>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|createjournal|Crea un diario de cambios USN.|
|m =\<MaxSize >|Especifica el tamaño máximo, en bytes, que NTFS asigna para el diario de cambios.|
|a=\<AllocationDelta>|Especifica el tamaño, en bytes, de asignación de memoria que se agrega al final y se quita del principio del diario de cambios.|
|\<VolumePath>|Especifica la letra de unidad (seguida de dos puntos).|
|deletejournal|Elimina o deshabilita un diario de cambios USN activo. **Precaución:** Eliminar el diario de cambios afecta al servicio de replicación de archivos (FRS) y el servicio de indización, ya que requeriría estos servicios para realizar un examen completo (y que requieren mucho tiempo) del volumen. Esto afecta negativamente a su vez a la replicación de SYSVOL de FRS y la replicación entre DFS vínculo alternativos mientras el volumen es que se vuelve a examinar.|
|/d|Deshabilita un diario de cambios USN activo y devuelve el control de entrada/salida (E/S) mientras se está deshabilitando el diario de cambios.|
|/n|Deshabilita un diario de cambios USN activo y devuelve el control de E/S solo después de que el diario de cambios está deshabilitado.|
|enablerangetracking|Permite el intervalo de escritura de USN de seguimiento para un volumen.|
|c=\<chunk-size>|Especifica el tamaño del fragmento para realizar un seguimiento en un volumen.|
|s=\<file-size-threshold>|Especifica el umbral de tamaño de archivo para el intervalo de seguimiento.|
|enumdata|Enumera y muestra las entradas del diario de cambios entre dos de los límites especificados.|
|\<FileRef>|Especifica la posición ordinal dentro de los archivos en el volumen en el que la enumeración se va a comenzar.|
|\<LowUSN>|Especifica el límite inferior del intervalo de valores USN utilizado para filtrar los registros que se devuelven. Solo los registros cuyo último cambio diario USN es entre o igual que el *usnBajo* y *usnAlto* se devuelven los valores de miembro.|
|\<UsnAlto >|Especifica el límite superior de los valores de intervalo de USN usados para filtrar los archivos que se devuelven.|
|queryjournal|Consulta los datos de USN de un volumen para recopilar información sobre el diario de cambios actual, sus registros y su capacidad.|
|ReadData|Lee los datos USN de un archivo.|
|\<FileName>|Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo: C:\documents\filename.txt|
|readjournal|Lee las entradas de USN en el diario USN.|
|minver=\<number>|Versión principal mínima de USN_RECORD para devolver. Valor predeterminado = 2.|
|maxver=\<number>|Versión principal de máximo de USN_RECORD para devolver. Valor predeterminado = 4.|
|startusn =\<número USN >|USN para empezar a leer el diario USN de. Valor predeterminado = 0.|


## <a name="remarks"></a>Comentarios

-   Sobre el diario de cambios USN

    El diario de cambios USN proporciona un registro persistente de todos los cambios realizados en los archivos del volumen. Como se agregan, eliminan y modificar los archivos, directorios y otros objetos NTFS, NTFS escribe registros en el diario de cambios USN, uno para cada volumen en el equipo. Cada registro indica el tipo de cambio y el objeto cambiado. Nuevos registros se anexan al final de la secuencia.

    Los programas pueden consultar el diario de cambios USN para determinar todas las modificaciones realizadas en un conjunto de archivos. El diario de cambios USN es mucho más eficaz que la comprobación de las marcas de tiempo o registrarse para notificaciones de archivo. El diario de cambios USN es habilitar y utilizar los servicios de Index Server, servicio de replicación de archivos (FRS), servicios de instalación remota (RIS) y el almacenamiento remoto.

-   Uso de **createjournal**

    Si ya existe un diario de cambios en un volumen, el **createjournal** parámetro actualiza el diario de cambios *MaxSize* y *diferenciaDeAsignación* parámetros. Esto le permite expandir el número de registros que mantiene un diario activo sin necesidad de deshabilitarlo.

-   Uso de **m**

    El diario de cambios puede crecer superior a este valor de destino, pero se trunca el diario de cambios en el siguiente punto de comprobación NTFS a menos que este valor. NTFS examina el diario de cambios y se recorta cuando su tamaño supera el valor de *MaxSize* más el valor de *diferenciaDeAsignación*. En los puntos de control NTFS, el sistema operativo escribe registros en el archivo de registro NTFS que permiten a NTFS determinar qué procesamiento es necesario para recuperarse de un error.

-   Uso de **un**

    El diario de cambios puede crecer más allá de la suma de los valores de *MaxSize* y *diferenciaDeAsignación* antes de que se va a recortar.

-   Uso de **deletejournal**

    Eliminar o deshabilitar un diario de cambios activos es muy lento, porque el sistema debe tener acceso a todos los registros de la tabla maestra de archivos (MFT) y establece el último atributo USN en 0 (cero). Este proceso puede tardar varios minutos y puede continuar una vez reiniciado el sistema, si es necesario reiniciar. Durante este proceso, el diario de cambios no se considera activo ni está deshabilitada. Mientras el sistema deshabilita el diario, no se puede acceder a él, y todas las operaciones de diario devuelven errores. Debe usar extremar las precauciones al deshabilitar un diario activo, ya que afecta negativamente a otras aplicaciones que usan el diario.

## <a name="BKMK_examples"></a>Ejemplos
Para crear un USN diario de cambios en la unidad C, escriba:

```
fsutil usn createjournal m=1000 a=100 c:
```

Para eliminar un activo USN diario de cambios en la unidad C, escriba:

```
fsutil usn deletejournal /d c:
```

Para habilitar el intervalo de seguimiento con un tamaño de fragmento especificado y el umbral de tamaño de archivo, escriba:

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

Para enumerar y mostrar las entradas del diario de cambios entre dos de los límites especificados en la unidad C, escriba:

```
fsutil usn enumdata 1 0 1 c:
```

Para consultar datos USN de un volumen en la unidad C, escriba:

```
fsutil usn queryjournal c:
```

Para leer los datos USN de un archivo en la carpeta \Temp en la unidad C, escriba:

```
fsutil usn readdata c:\temp\sample.txt
```

Para leer el diario USN con un USN de inicio específica, escriba:

```
fsutil usn readjournal startusn=0xF00
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


