---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: Fsutil USN
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b62d031c547f140ac5008af20a9e0ee4bcecc919
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376792"
---
# <a name="fsutil-usn"></a>Fsutil USN
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra el diario de cambios del número de secuencias actualizadas (USN).

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
|m = \<MaxSize >|Especifica el tamaño máximo, en bytes, que NTFS asigna para el diario de cambios.|
|a = \<AllocationDelta >|Especifica el tamaño, en bytes, de la asignación de memoria que se agrega al final y se quita del principio del diario de cambios.|
|@no__t 0VolumePath >|Especifica la letra de unidad (seguida de dos puntos).|
|deletejournal|Elimina o deshabilita un diario de cambios USN activo. **Atención:** La eliminación del diario de cambios afecta al servicio de replicación de archivos (FRS) y al servicio de indexación, ya que requeriría que estos servicios realizaran un examen completo (y lento) del volumen. Esto, a su vez, afecta negativamente a la replicación SYSVOL de FRS y la replicación entre alternativas de vínculo DFS mientras se vuelve a examinar el volumen.|
|/d|Deshabilita un diario de cambios USN activo y devuelve un control de entrada/salida (e/s) mientras se deshabilita el diario de cambios.|
|/n|Deshabilita un diario de cambios USN activo y devuelve el control de e/s solo después de deshabilitar el diario de cambios.|
|enablerangetracking|Habilita el seguimiento del intervalo de escritura USN para un volumen.|
|c = \<chunk-size >|Especifica el tamaño del fragmento del que se va a realizar el seguimiento en un volumen.|
|s = \<file-size-Threshold >|Especifica el umbral de tamaño de archivo para el seguimiento del intervalo.|
|enumdata|Enumera y enumera las entradas del diario de cambios entre dos límites especificados.|
|@no__t 0FileRef >|Especifica la posición ordinal dentro de los archivos en el volumen en el que va a comenzar la enumeración.|
|@no__t 0LowUSN >|Especifica el límite inferior del intervalo de valores de USN utilizados para filtrar los registros que se devuelven. Solo se devuelven los registros cuyo USN del último diario de cambios está entre o igual que los valores de miembro *LowUSN* y *HighUSN* .|
|@no__t 0HighUSN >|Especifica el límite superior del intervalo de valores de USN utilizados para filtrar los archivos que se devuelven.|
|queryjournal|Consulta los datos de USN de un volumen para recopilar información sobre el diario de cambios actual, sus registros y su capacidad.|
|ReadData|Lee los datos USN de un archivo.|
|\<Nombre de archivo >|Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo: C:\documents\filename.txt|
|readjournal|Lee los registros USN en el diario USN.|
|Minver = \<number >|Versión principal mínima de USN_RECORD que se va a devolver. Valor predeterminado = 2.|
|maxver = \<number >|Versión principal máxima de USN_RECORD que se va a devolver. Valor predeterminado = 4.|
|startusn = \<USN número >|USN del que se va a empezar a leer el diario USN. Valor predeterminado = 0.|


## <a name="remarks"></a>Comentarios

-   Acerca del diario de cambios de USN

    El diario de cambios USN proporciona un registro persistente de todos los cambios realizados en los archivos del volumen. A medida que se agregan, eliminan y modifican archivos, directorios y otros objetos NTFS, NTFS escribe registros en el diario de cambios USN, uno para cada volumen del equipo. Cada registro indica el tipo de cambio y el objeto cambiado. Los nuevos registros se anexan al final de la secuencia.

    Los programas pueden consultar el diario de cambios USN para determinar todas las modificaciones realizadas en un conjunto de archivos. El diario de cambios USN es mucho más eficaz que comprobar las marcas de tiempo o registrar las notificaciones de archivo. El diario de cambios USN está habilitado y utilizado por el servicio de indización, el servicio de replicación de archivos (FRS), los servicios de instalación remota (RIS) y el almacenamiento remoto.

-   Usar **createjournal**

    Si ya existe un diario de cambios en un volumen, el parámetro **createjournal** actualiza los parámetros *MaxSize* y *AllocationDelta* del diario de cambios. Esto le permite ampliar el número de registros que mantiene un diario activo sin tener que deshabilitarlo.

-   Usar **m**

    El diario de cambios puede aumentar el tamaño de este valor de destino, pero el diario de cambios se trunca en el siguiente punto de comprobación de NTFS a menos de este valor. NTFS examina el diario de cambios y lo recorta cuando su tamaño supera el valor de *MaxSize* más el valor de *AllocationDelta*. En los puntos de comprobación de NTFS, el sistema operativo escribe registros en el archivo de registro de NTFS que permiten a NTFS determinar qué procesamiento es necesario para recuperarse de un error.

-   Usar **un**

    El diario de cambios puede aumentar hasta más de la suma de los valores de *MaxSize* y *AllocationDelta* antes de que se recorte.

-   Usar **deletejournal**

    Eliminar o deshabilitar un diario de cambios activo lleva mucho tiempo, ya que el sistema debe tener acceso a todos los registros de la tabla de archivos maestra (MFT) y establecer el último atributo USN en 0 (cero). Este proceso puede tardar varios minutos y puede continuar después de que se reinicie el sistema, en caso de que sea necesario reiniciar. Durante este proceso, el diario de cambios no se considera activo ni está deshabilitado. Mientras el sistema está deshabilitando el diario, no se puede tener acceso a él y todas las operaciones del diario devuelven errores. Debe extremar las precauciones al deshabilitar un diario activo, ya que afecta negativamente a otras aplicaciones que utilizan el diario.

## <a name="BKMK_examples"></a>Example
Para crear un diario de cambios USN en la unidad C, escriba:

```
fsutil usn createjournal m=1000 a=100 c:
```

Para eliminar un diario de cambios de USN activo en la unidad C, escriba:

```
fsutil usn deletejournal /d c:
```

Para habilitar el seguimiento de intervalo con un tamaño de fragmento y un umbral de tamaño de archivo especificados, escriba:

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

Para enumerar y mostrar las entradas del diario de cambios entre dos límites especificados en la unidad C, escriba:

```
fsutil usn enumdata 1 0 1 c:
```

Para consultar los datos USN de un volumen en la unidad C, escriba:

```
fsutil usn queryjournal c:
```

Para leer los datos USN de un archivo en la carpeta \temp de la unidad C, escriba:

```
fsutil usn readdata c:\temp\sample.txt
```

Para leer el diario USN con un USN de inicio específico, escriba:

```
fsutil usn readjournal startusn=0xF00
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


