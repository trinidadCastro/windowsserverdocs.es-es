---
title: fsutil behavior
description: Artículo de referencia para el comando fsutil Behavior, que consulta o establece el comportamiento del volumen NTFS.
manager: dmoss
ms.author: toklima
author: toklima
ms.topic: reference
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: c677836b70416f1d02508436730a6c9d2841e849
ms.sourcegitcommit: b39ea3b83280f00e5bb100df0dc8beaf1fb55be2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94520478"
---
# <a name="fsutil-behavior"></a>fsutil behavior

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Consulta o establece el comportamiento del volumen NTFS, que incluye:

- Crear los nombres de archivo de longitud de caracteres 8,3.

- Extender el uso de caracteres en nombres de archivo cortos de 8,3 de longitud de caracteres en volúmenes NTFS.

- Actualización de la marca de **tiempo del último acceso** cuando los directorios se enumeran en volúmenes NTFS.

- La frecuencia con la que los eventos de cuota se escriben en el registro del sistema y en los niveles de caché de memoria de bloque no paginado de NTFS y grupo paginado de NTFS.

- Tamaño de la zona de tabla de archivos maestra (zona MFT).

- Eliminación silenciosa de datos cuando el sistema encuentra daños en un volumen NTFS.

- Notificación de eliminación de archivo (también conocida como recorte o desasignación).

## <a name="syntax"></a>Sintaxis

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<volumepath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <value> | [<volumepath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <frequency> | symlinkevaluation <symboliclinktype> | disabledeletenotify {1|0}}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| Query | Consulta los parámetros de comportamiento del sistema de archivos. |
| set | Cambia los parámetros de comportamiento del sistema de archivos. |
| allowextchar `{1|0}` | Permite ( **1** ) o no permite ( **0** ) caracteres del juego de caracteres extendido (incluidos los caracteres diacríticos) que se van a usar en nombres de archivo cortos de longitud de caracteres de 8,3 en volúmenes NTFS.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| Bugcheckoncorrupt `{1|0}` | Permite ( **1** ) o no permite la generación ( **0** ) de una comprobación de errores cuando hay daños en un volumen NTFS. Esta característica se puede usar para impedir que NTFS elimine los datos de forma silenciosa cuando se usa con la característica NTFS Self-Healing.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| disable8dot3 [ <volumepath> ] `{1|0}` | Deshabilita ( **1** ) o habilita ( **0** ) la creación de nombres de archivo 8,3 de longitud de caracteres en volúmenes FAT y con formato NTFS. Opcionalmente, prefijo con el *VolumePath* especificado como nombre de unidad seguido de un signo de dos puntos o un GUID. |
| disablecompression `{1|0}` | Deshabilita **(1)** o habilita **(0)** la compresión NTFS.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| disablecompressionlimit `{1|0}` | Deshabilita **(1)** o habilita **(0)** el límite de compresión NTFS en el volumen NTFS. Cuando un archivo comprimido alcanza un cierto nivel de fragmentación, en lugar de no tener que extender el archivo, NTFS deja de comprimir las extensiones adicionales del archivo. Esto se hace para permitir que los archivos comprimidos sean más grandes de lo normal. Al establecer este valor en **true** , se deshabilita esta característica, lo que limita el tamaño de los archivos comprimidos en el sistema. No se recomienda deshabilitar esta característica.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| disableencryption `{1|0}` | Deshabilita **(1)** o habilita **(0)** el cifrado de carpetas y archivos en volúmenes NTFS.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| disablefilemetadataoptimization `{1|0}` | Deshabilita **(1)** o habilita **(0)** la optimización de metadatos de archivo. NTFS tiene un límite en el número de extensiones que puede tener un archivo determinado. Los archivos comprimidos y dispersos pueden llegar a estar muy fragmentados. De forma predeterminada, NTFS compacta periódicamente sus estructuras de metadatos internas para permitir más archivos fragmentados. Al establecer este valor en **true** , se deshabilita esta optimización interna. No se recomienda deshabilitar esta característica.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| disablelastaccess `{1|0}` | Deshabilita ( **1** ) o habilita ( **0** ) actualizaciones de la marca de hora del último acceso en cada directorio cuando los directorios aparecen en un volumen NTFS.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| disablespotcorruptionhandling `{1|0}` | Deshabilita **(1)** o habilita **(0)** marcar el control de daños. También permite a los administradores del sistema ejecutar CHKDSK para analizar el estado de un volumen sin dejarlo sin conexión. No se recomienda deshabilitar esta característica.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| disabletxf `{1|0}` | Deshabilita **(1)** o habilita **(0)** TxF en el volumen NTFS especificado. TxF es una característica de NTFS que proporciona la semántica de transacciones a las operaciones del sistema de archivos. TxF está actualmente en desuso, pero la funcionalidad todavía está disponible. No se recomienda deshabilitar esta característica en el volumen C:.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| disablewriteautotiering `{1|0}` | Deshabilita la lógica de niveles automáticos de ReFS v2 para volúmenes en capas.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| encryptpagingfile `{1|0}` | Cifra ( **1** ) o no cifra ( **0** ) el archivo de paginación de memoria en el sistema operativo Windows.<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| mftzone `<value>` | Establece el tamaño de la zona MFT y se expresa como un múltiplo de 200 MB de unidades. Establezca el *valor* en un número de **1** (el valor predeterminado es 200 MB) en **4** (el máximo es 800 MB).<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| MemoryUsage `<value>` | Configura los niveles de caché interna de la memoria de grupo paginada de NTFS y la memoria de grupo no paginada de NTFS. Establezca en **1** o **2**. Cuando se establece en **1** (valor predeterminado), NTFS usa la cantidad predeterminada de memoria de grupo paginada. Cuando se establece en **2** , NTFS aumenta el tamaño de las listas de direcciones y los umbrales de memoria. (Una lista de direcciones es un grupo de búferes de memoria de tamaño fijo que el kernel y los controladores de dispositivos crean como memoria caché de memoria privada para las operaciones del sistema de archivos, como la lectura de un archivo).<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| quotanotify `<frequency>` | Configura la frecuencia con que se registran las infracciones de cuota NTFS en el registro del sistema. Los valores válidos para están en el intervalo comprendido entre **0 y 4294967295**. La frecuencia predeterminada es de **3600** segundos (una hora).<p>Debe reiniciar el equipo para que este parámetro surta efecto. |
| symlinkevaluation `<symboliclinktype>` | Controla el tipo de vínculos simbólicos que se pueden crear en un equipo. Las opciones válidas son:<ul><li>**1** : vínculos simbólicos locales a locales, `L2L:{0|1}`</li><li>**2** -vínculos simbólicos locales a remotos, `L2R:{1|0}`</li><li>**3** -vínculos simbólicos remotos a locales, `R2L:{1|0}`</li><li>**4** -vínculos simbólicos remotos a remotos `R2R:{1|0}`</li></ul> |
| disabledeletenotify | Deshabilita ( **1** ) o habilita ( **0** ) las notificaciones de eliminación. Las notificaciones de eliminación (también conocidas como recorte o desasignación) son una característica que notifica al dispositivo de almacenamiento subyacente de los clústeres que se han liberado debido a una operación de eliminación de archivos. Además:<ul><li>En los sistemas que usan ReFS V2, Trim está deshabilitado de forma predeterminada.</li><li>En los sistemas que usan ReFS v1, Trim está habilitado de forma predeterminada.</li><li>En los sistemas que usan NTFS, Trim está habilitado de forma predeterminada a menos que un administrador lo deshabilite.</li><li>Si la unidad de disco duro o SAN informa de que no admite Trim, la unidad de disco duro y las San no obtienen notificaciones de recorte.</li><li>La habilitación o deshabilitación de no requiere un reinicio.</li><li>Trim es efectivo cuando se emite el siguiente comando de desasignación.</li><li>La e/s de Inflight existente no se ve afectada por el cambio del registro.</li><li>No es necesario reiniciar el servicio al habilitar o deshabilitar Trim.</li></ul> |

#### <a name="remarks"></a>Observaciones

- La zona MFT es un área reservada que permite que la tabla de archivos maestros (MFT) se expanda según sea necesario para evitar la fragmentación de MFT. Si el tamaño de archivo promedio en el volumen es de 2 KB o menos, puede ser beneficioso establecer el valor de **mftzone** en **2**. Si el tamaño de archivo promedio en el volumen es de 1 KB o menos, puede ser beneficioso establecer el valor de **mftzone** en **4**.

- Cuando **disable8dot3** se establece en **0** , cada vez que se crea un archivo con un nombre de archivo largo, NTFS crea una segunda entrada de archivo que tiene un nombre de archivo de longitud 8,3. Cuando NTFS crea archivos en un directorio, debe buscar los nombres de archivo de longitud de caracteres 8,3 asociados a los nombres de archivo largos. Este parámetro actualiza la clave del registro **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation** .

- El parámetro **allowextchar** actualiza la clave del registro **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name** .

- El parámetro **disablelastaccess** reduce el impacto de las actualizaciones de registro en la última marca de **tiempo de acceso** en los archivos y directorios. Al deshabilitar la característica de **hora de último acceso** , se mejora la velocidad de acceso a archivos y directorios. Este parámetro actualiza la clave del registro **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate** .

    **Notas:**

    - Las consultas de **hora del último acceso** basado en archivos son precisas incluso si todos los valores del disco no están actualizados. NTFS devuelve el valor correcto en las consultas porque el valor preciso se almacena en memoria.

    - Una hora es la cantidad máxima de tiempo que NTFS puede aplazar la actualización de la **hora del último acceso en el** disco. Si NTFS actualiza otros atributos de archivo, como la hora de la **última modificación** , y hay una actualización de **hora de último acceso** pendiente, NTFS actualiza la **hora del último acceso** con las demás actualizaciones sin afectar al rendimiento adicional.

    - El parámetro **disablelastaccess** puede afectar a programas como copia de seguridad y almacenamiento remoto, que se basan en esta característica.

- Aumentar la memoria física no siempre aumenta la cantidad de memoria del grupo paginado disponible para NTFS. Establecer **MemoryUsage** en **2** eleva el límite de memoria del grupo paginado. Esto puede mejorar el rendimiento si el sistema está abriendo y cerrando muchos archivos en el mismo conjunto de archivos y aún no usa grandes cantidades de memoria del sistema para otras aplicaciones o para la memoria caché. Si el equipo ya usa grandes cantidades de memoria del sistema para otras aplicaciones o para la memoria caché, al aumentar el límite de memoria de grupo paginada de NTFS y de bloque no paginado se reduce la memoria de Grupo disponible para otros procesos. Esto podría reducir el rendimiento general del sistema. Este parámetro actualiza la clave del registro **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage** .

- El valor especificado en el parámetro **mftzone** es una aproximación del tamaño inicial del MFT más la zona MFT en un nuevo volumen y se establece en el momento del montaje de cada sistema de archivos. A medida que se usa el espacio en el volumen, NTFS ajusta el espacio reservado para el crecimiento futuro de la MFT. Si la zona MFT ya es grande, el tamaño completo de la zona MFT no se reserva de nuevo. Dado que la zona MFT se basa en el intervalo contiguo más allá del final de la MFT, se reduce a medida que se utiliza el espacio.

    El sistema de archivos no determina la nueva ubicación de la zona MFT hasta que la zona MFT actual se use por completo. Tenga en cuenta que esto nunca se produce en un sistema típico.

- Algunos dispositivos pueden experimentar una degradación del rendimiento cuando la característica de notificación de eliminación está activada. En este caso, use la opción **disabledeletenotify** para desactivar la característica de notificación.

### <a name="examples"></a>Ejemplos

Para consultar el comportamiento Disable 8.3 Name para un volumen de disco especificado con el GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, escriba:

```
fsutil behavior query disable8dot3 volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

También puede consultar el comportamiento del nombre 8.3 mediante el subcomando **8dot3name** .

Para consultar el sistema y ver si el recorte está habilitado o no, escriba:

```
fsutil behavior query DisableDeleteNotify
```

Esto produce una salida similar a la siguiente:

```
NTFS DisableDeleteNotify = 1
ReFS DisableDeleteNotify is not currently set
```

Para invalidar el comportamiento predeterminado de TRIM (disabledeletenotify) para ReFS V2, escriba:

```
fsutil behavior set disabledeletenotify ReFS 0
```

Para invalidar el comportamiento predeterminado de TRIM (disabledeletenotify) para NTFS y ReFS v1, escriba:

```
fsutil behavior set disabledeletenotify 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [fsutil 8dot3name](fsutil-8dot3name.md)
