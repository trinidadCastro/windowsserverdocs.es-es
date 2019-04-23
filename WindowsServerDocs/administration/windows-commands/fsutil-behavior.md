---
title: Comportamiento de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: 4593739f25c356e72ea39947c67f3e1301573137
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838276"
---
# <a name="fsutil-behavior"></a>Comportamiento de fsutil

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Las consultas o establece el comportamiento de volumen NTFS, que incluye:

-   La creación de nombres de archivo de longitud de caracteres con formato 8.3

-   Uso de caracteres extendidos en los nombres de archivo cortos de longitud de caracteres 8.3 en volúmenes NTFS

-   La actualización de la marca de hora del último acceso cuando se enumeran los directorios en los volúmenes NTFS

-   La frecuencia con la cuota de qué eventos se escriben en el registro del sistema y a NTFS paginado grupo y niveles de caché de memoria de bloque no paginado de NTFS

-   El tamaño de la zona de tabla maestra de archivos (MFT zona)

-   Eliminación silenciosa de datos cuando el sistema detecta daños en un volumen NTFS.

-   Notificación de eliminación de archivos (también conocido como recortar o quitar la asignación)

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<VolumePath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <Value> | [<VolumePath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <Frequency> | symlinkevaluation <SymbolicLinkType> | disabledeletenotify {1|0}}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|query|Consulta los parámetros de comportamiento del sistema de archivos.|
|set|Cambia los parámetros de comportamiento del sistema de archivos.|
|allowextchar {1&#124;0}|Permite (**1**) o lo deniega (**0**) caracteres extendidos de conjunto (incluidos los caracteres de signos diacríticos) que debe usarse en los nombres de archivo cortos de longitud de caracteres 8.3 en volúmenes NTFS.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|Bugcheckoncorrupt {1&#124;0}|Permite (**1**) o lo deniega (**0**) la generación de una comprobación de errores cuando hay daños en un volumen NTFS. Esta característica puede usarse para evitar que NTFS en modo silencioso eliminen datos cuando se usa con la característica de NTFS de recuperación automática.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.<br /><br />Este parámetro se aplica a:  Windows Server 2008 R2 y Windows 7.|
|disable8dot3 [<VolumePath>] {1&#124;0}|Deshabilita (**1**) o habilita (**0**) la creación de nombres de archivo de longitud de caracteres 8.3 en volúmenes con formato NTFS y FAT. Si lo desea, con el prefijo del *VolumePath* especificado como un nombre de la unidad seguido por un signo de dos puntos o un GUID.|
|disablecompression {1&#124;0}|Deshabilita **(1)** o habilita **(0)** compresión NTFS.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|disablecompressionlimit {1&#124;0}| Deshabilita **(1)** o habilita **(0)** límite de la compresión NTFS en volúmenes NTFS. Cuando un archivo comprimido alcanza un cierto nivel de fragmentación, en lugar de no se puede ampliar el archivo, NTFS detiene la compresión de las extensiones adicionales del archivo. Esto se hace para permitir que los archivos comprimidos sean mayores de lo que normalmente serían. Establezca este valor en TRUE, se deshabilita esta característica que limita el tamaño de los archivos en el sistema comprimidos. No se recomienda deshabilitar esta característica.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|DisableEncryption {1&#124;0}|Deshabilita **(1)** o habilita **(0)** el cifrado de carpetas y archivos en volúmenes NTFS.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|disablefilemetadataoptimization {1&#124;0}|Deshabilita **(1)** o habilita **(0)** optimización de los metadatos de archivo. NTFS tiene un límite en extensiones cuántas puede tener un archivo determinado. Comprimidos y archivos de reservas pueden llegar a ser muy fragmentados. De forma predeterminada, NTFS compacta periódicamente sus estructuras de metadatos interno para permitir que los archivos más fragmentados. Establecer este valor en TRUE, se deshabilita esta optimización interna. No se recomienda deshabilitar esta característica.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|disablelastaccess {1&#124;0}|Deshabilita (**1**) o habilita (**0**) se actualiza a la última marca de tiempo de acceso en cada directorio cuando se enumeran los directorios en un volumen NTFS.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|disablespotcorruptionhandling {1&#124;0}|Deshabilita **(1)** o habilita **(0)** control detectar daños. Una de las nuevas características introducidas en Windows 8 y Windows Server 2012 es una nueva forma de alta disponibilidad de CHKDSK. Esta característica permite a los administradores del sistema ejecute CHKDSK para analizar el estado de un volumen sin interrumpir la conexión. No se recomienda deshabilitar esta característica.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|disabletxf {1&#124;0}|Deshabilita **(1)** o habilita **(0)** txf en el volumen NTFS especificado. TxF es una característica NTFS que proporciona la transacción como la semántica de las operaciones de sistema de archivos. TxF actualmente es deprected aunque la funcionalidad aún está disponible. No se recomienda deshabilitar esta característica en el volumen C:.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|disablewriteautotiering {1&#124;0}|Deshabilita la lógica de organización en capas automática de ReFS v2 para volúmenes en capas.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|encryptpagingfile {1&#124;0}|Cifra (**1**) o no cifrar (**0**) el archivo de paginación de memoria en el sistema operativo de Windows.<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|mftzone <Value>|Establece el tamaño de la zona MFT y se expresa como un múltiplo de unidades de 200MB. Establecer *valor* a un número comprendido entre **1** (valor predeterminado es 200 MB) a **4** (máximo es de 800 MB).<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|memoryusage <Value>|Configura los niveles de caché interna de memoria de bloque paginado de NTFS y de memoria de bloque no paginado de NTFS. Establecido en **1** o **2**. Cuando se establece en **1** (valor predeterminado), NTFS usa la cantidad de memoria de grupo paginada predeterminada. Cuando se establece en **2**, NTFS aumenta el tamaño de sus listas de direcciones y los umbrales de memoria. (Una lista de direcciones es un grupo de búferes de memoria de tamaño fijo que el kernel y controladores de dispositivos se crean como caché de memoria privada para las operaciones de sistema de archivos, como la lectura de un archivo).<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|quotanotify <Frequency>|Se configura con qué frecuencia se notifican las infracciones de cuota NTFS en el registro del sistema. Los valores válidos para están en el intervalo de 0 a 4294967295. La frecuencia predeterminada es de 3600 segundos (una hora).<br /><br />Debe reiniciar el equipo para este parámetro surta efecto.|
|symlinkevaluation <SymbolicLinkType>|Controla el tipo de vínculos simbólicos que se pueden crear en un equipo. Las opciones válidas son:<br /><br />1.  Local a locales vínculos simbólicos, L2L: {0&#124;1}<br />2.  Local a remotos vínculos simbólicos, L2R: {0&#124;1}<br />3.  Remoto a locales vínculos simbólicos, R2R: {0&#124;1}<br />4.  Remoto a los vínculos simbólicos remotos, R2L: {0&#124;1}|
|DisableDeleteNotify|Deshabilita \( **1** \) o habilita \( **0** \) eliminar las notificaciones. Eliminar notificación \(también conocido como recortar o anular la asignación de\) es una característica que notifica el dispositivo de almacenamiento subyacente de los clústeres que se han liberado debido a un archivo de la operación de eliminación. Además:<br /><br />-Para sistemas con ReFS v2, recorte está deshabilitada de forma predeterminada. Se aplica a Windows Server 2016.<br />-Para sistemas con ReFS v1, recorte está habilitado de forma predeterminada. Se aplica a Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016.<br />-Para sistemas que utilicen NTFS, recorte está habilitado de forma predeterminada a menos que un administrador lo deshabilita.<br />-Si la unidad de disco duro o SAN informa de que no admite recorte, la unidad de disco duro y SAN no obtener notificaciones de recorte.<br />-Habilitar o deshabilitar no requiere un reinicio.<br />-Trim es eficaz al anular la asignación el siguiente comando se emite.<br />-Existentes en proceso de E/S no se ven afectadas por el cambio del registro.<br />-No requiere ningún reinicio del servicio al habilitar o deshabilitar el recorte.<br /><br />Este parámetro se incorporó en Windows Server 2008 R2 y Windows 7. | 

## <a name="remarks"></a>Comentarios

-   La zona MFT es un área reservada que permite a la tabla maestra de archivos (MFT) para expandir según sea necesario para evitar la fragmentación de MFT. Si el tamaño promedio de los archivos en el volumen es de 2 KB o menos, puede ser útil establecer el **mftzone** valor a 2. Si el tamaño promedio de los archivos en el volumen es de 1 KB o menos, puede ser útil establecer el **mftzone** valor a 4.

-   Cuando **disable8dot3** está establecido en **0**, cada vez cree un archivo con un nombre de archivo largo, NTFS crea una segunda entrada de archivo que tiene un nombre de 8.3 archivo de longitud de caracteres. Cuando NTFS crea archivos en un directorio, debe buscar los nombres de archivo de longitud de caracteres con formato 8.3 que están asociados con los nombres de archivo largos. Este parámetro actualiza el **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation** clave del registro.

-   El **allowextchar** actualizaciones de parámetros la **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name** clave del registro.

-   El **disablelastaccess** parámetro reduce el impacto de las actualizaciones del registro en la marca de hora del último acceso en archivos y directorios. Deshabilitar la **hora del último acceso** característica mejora la velocidad de acceso de archivo y directorio. Este parámetro actualiza el **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate** clave del registro.

    Notas:

    -   Basado en archivos **hora del último acceso** consultas son precisas, incluso si todos los valores en el disco no están actualizados. NTFS devuelve el valor correcto en las consultas porque el valor exacto se almacena en memoria.

    -   Es la cantidad máxima de tiempo que NTFS aplaza la actualización de una hora **hora del último acceso** en el disco. Si NTFS actualiza otros atributos de archivo como **hora de última modificación**y un **hora del último acceso** actualización está pendiente, las actualizaciones NTFS **hora del último acceso** con el resto de actualizaciones sin impacto de rendimiento adicionales.

    -   El **disablelastaccess** parámetros pueden afectar a programas como copia de seguridad y almacenamiento remoto que se basan en esta característica.

-   Aumento de la memoria física no siempre aumenta la cantidad de memoria paginada para NTFS. Establecer **memoryusage** a **2** eleva el límite de memoria de bloque paginado. Esto puede mejorar el rendimiento si su sistema está abriendo y cerrando muchos archivos en el mismo archivo establece y ya no usa grandes cantidades de memoria del sistema para otras aplicaciones o para la memoria caché. Si el equipo ya está usando grandes cantidades de memoria del sistema para otras aplicaciones o para la memoria caché, aumentar el límite de NTFS paginado y memoria de bloque no paginado reduce la memoria disponible para otros procesos. Esto podría reducir el rendimiento general del sistema. Este parámetro actualiza el **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage** clave del registro.

-   El valor especificado en el **mftzone** parámetro es una aproximación del tamaño inicial de la MFT más la zona MFT en un volumen nuevo y se establece en tiempo de montaje para cada sistema de archivos. A medida que se usa el espacio en el volumen, NTFS ajusta el espacio reservado para el crecimiento futuro de MFT. Si la zona MFT ya es grande, el tamaño de la zona MFT completo no está reservado a intentarlo. Dado que la zona MFT se basa en el intervalo contiguo más allá del final de la MFT, se reduce a medida se utiliza el espacio.

    El sistema de archivos no determina la nueva ubicación de la zona MFT hasta que la zona MFT actual se usa por completo. Tenga en cuenta que esto nunca se produce en un sistema típico.

-   Algunos dispositivos pueden experimentar una degradación del rendimiento cuando está activada la característica de notificación de eliminación. En este caso, utilice el **disabledeletenotify** opción para desactivar la característica de notificación.

### <a name="BKMK_examples"></a>Ejemplos
Para consultar el comportamiento de deshabilitación 8.3 para un volumen de disco especificado con el GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, escriba:

```
fsutil behavior query disable8dot3 Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

También puede consultar el comportamiento de nombre 8.3 utilizando el **8dot3name** subcomando.

Para consultar el sistema para ver si TRIM está habilitada o no, escriba:

```
fsutil behavior query DisableDeleteNotify
```
Esto da como resultado una salida similar al siguiente:

    NTFS DisableDeleteNotify = 1
    ReFS DisableDeleteNotify is not currently set

Para invalidar el comportamiento predeterminado del RECORTE \(disabledeletenotify\) para ReFS v2, escriba:

```
fsutil behavior set DisableDeleteNotify ReFS 0
```

Para invalidar el comportamiento predeterminado del RECORTE \(disabledeletenotify\) para NTFS y ReFS v1, escriba:
```
fsutil behavior set DisableDeleteNotify 1
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[fsutil 8dot3name](Fsutil-8dot3name.md)


