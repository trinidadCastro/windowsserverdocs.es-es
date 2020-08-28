---
title: robocopy
description: Artículo de referencia del comando Robocopy, que copia datos de archivos de una ubicación a otra.
ms.topic: reference
ms.assetid: d4c6e8e9-fcb3-4a4a-9d04-2d8c367b6354
author: jasongerend
ms.author: jgerend
manager: lizapo
ms.date: 06/07/2020
ms.openlocfilehash: b149294436c78c3c9c223973fb3e9b423ff3dfd0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036213"
---
# <a name="robocopy"></a>robocopy

Copia los datos de archivo de una ubicación a otra.

## <a name="syntax"></a>Sintaxis

```
robocopy <source> <destination> [<file>[ ...]] [<options>]
```

Por ejemplo, para copiar un archivo llamado *yearly-Report. mov* de *c:\Informes* en un recurso compartido de archivos * \\ marketing\videos* mientras se habilita el multithreading para obtener un mayor rendimiento (con el parámetro **/MT** ) y la capacidad de reiniciar la transferencia en caso de que se interrumpa (con el parámetro **/z** ), escriba:

```dos
robocopy c:\reports '\\marketing\videos' yearly-report.mov /mt /z
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<source>` | Especifica la ruta de acceso del directorio de origen. |
| `<destination>` | Especifica la ruta de acceso del directorio de destino. |
| `<file>` | Especifica el archivo o los archivos que se van a copiar. Se admiten los caracteres comodín (**&#42;** o **?**). Si no se especifica este parámetro, `*.` se usa como valor predeterminado. |
| `<options>` | Especifica las opciones que se van a usar con el comando **Robocopy** , incluidas las opciones de **copia**, **archivo**, **reintento**, **registro**y **trabajo** . |

#### <a name="copy-options"></a>Opciones de copia

| Opción | Descripción |
|--|--|
| /s | Copia subdirectorios. Esta opción excluye automáticamente los directorios vacíos. |
| /e | Copia subdirectorios. Esta opción excluye automáticamente los directorios vacíos. |
| nivel`<n>` | Copia solo los *n* niveles principales del árbol de directorio de origen. |
| /z | Copia los archivos en modo reiniciable. |
| /b | Copia los archivos en modo de copia de seguridad. |
| /zb | Utiliza el modo reiniciable. Si se deniega el acceso, esta opción utiliza el modo de copia de seguridad. |
| /efsraw | Copia todos los archivos cifrados en modo RAW de EFS. |
| /Copy`<copyflags>` | Especifica las propiedades de archivo que se van a copiar. Los valores válidos para esta opción son:<ul><li>**D** -datos</li><li>Atributos **a**</li><li>Marcas de tiempo **T**</li><li>**S** -lista de control de acceso (ACL) de NTFS</li><li>Información del propietario **O**</li><li>Información de auditoría de **U**</li></ul>El valor predeterminado de esta opción es **DAT** (datos, atributos y marcas de tiempo). |
| /dcopy:`<copyflags>`| Especifica qué copiar en los directorios. Los valores válidos para esta opción son:<ul><li>**D** -datos</li><li>Atributos **a**</li><li>Marcas de tiempo **T**</li></ul>El valor predeterminado de esta opción es **da** (datos y atributos). |
| /s | Copia los archivos con seguridad (equivalente a **/Copy: DATS**). |
| /copyall | Copia toda la información de archivo (equivalente a **/Copy: DATSOU**). |
| /nocopy | No copia ninguna información de archivo (útil con **/Purge**). |
| /secfix | Corrige la seguridad de los archivos en todos los archivos, incluso los omitidos. |
| /timfix | Corrige los tiempos de archivo en todos los archivos, incluso los omitidos. |
| /purge | Elimina los archivos y directorios de destino que ya no existen en el origen. El uso de esta opción con la opción **/e** y un directorio de destino permite no sobrescribir la configuración de seguridad del directorio de destino. |
| /mir | Refleja un árbol de directorios (equivalente a **/e** más **/Purge**). Al usar esta opción con la opción **/e** y un directorio de destino, se sobrescribe la configuración de seguridad del directorio de destino. |
| /mov | Mueve archivos y los elimina del origen una vez copiados. |
| /Move | Mueve archivos y directorios, y los elimina del origen una vez copiados. |
| /a +: [RASHCNET] | Agrega los atributos especificados a los archivos copiados. |
| /a-(: [RASHCNET] | Quita los atributos especificados de los archivos copiados. |
| /create | Solo crea un árbol de directorios y archivos de longitud cero. |
| /fat | Crea archivos de destino usando únicamente nombres de archivo FAT de longitud de caracteres 8,3. |
| /256 | Desactiva la compatibilidad con las rutas de acceso de más de 256 caracteres. |
| mes`<n>` | Supervisa el origen y vuelve a ejecutarse cuando se detectan más de *n* cambios. |
| /mot:`<m>` | Supervisa el origen y vuelve a ejecutarse en *m* minutos, si se detectan cambios. |
| /MT`[:n]` | Crea copias multiproceso con *n* subprocesos. *n* debe ser un entero comprendido entre 1 y 128. El valor predeterminado de *n* es 8. Para mejorar el rendimiento, redirija la salida mediante la opción **/log** .<p>El parámetro **/MT** no se puede usar con los parámetros **/IPG** y **/efsraw** . |
| /RH: hhmm-hhmm | Especifica los tiempos de ejecución cuando se pueden iniciar nuevas copias. |
| /PF | Comprueba los tiempos de ejecución por archivo (no por paso). |
| /IPG: n | Especifica el intervalo entre paquetes para el ancho de banda libre en líneas lentas. |
| /sl | No siga los vínculos simbólicos y, en su lugar, cree una copia del vínculo. |

> [!IMPORTANT]
> Al usar la opción de copia **/secfix** , especifique el tipo de información de seguridad que desea copiar mediante una de estas opciones de copia adicionales:
>
>- **/copyall**
>- **/Copy: o**
>- **/Copy: s**
>- **/Copy: u**
>- **/sec**

#### <a name="file-selection-options"></a>Opciones de selección de archivos

| Opción | Descripción |
|--|--|
| /a | Copia solo los archivos para los que se establece el atributo de **archivo** . |
| /m | Copia solo los archivos para los que se establece el atributo **Archive** y restablece el atributo **Archive** . |
| /IA`[RASHCNETO]` | Incluye solo los archivos para los que se establece cualquiera de los atributos especificados. |
| XA`[RASHCNETO]` | Excluye los archivos para los que se establece cualquiera de los atributos especificados. |
| /xf `<filename>[ ...]` | Excluye los archivos que coinciden con los nombres o rutas de acceso especificados. Se admiten los caracteres comodín (**&#42;** y **?**). |
| /xd `<directory>[ ...]` | Excluye los directorios que coinciden con los nombres y rutas de acceso especificados. |
| /xc | Excluye los archivos modificados. |
| /xn | Excluye los archivos más recientes. |
| /xo | Excluye los archivos más antiguos. |
| /xx | Excluye archivos y directorios adicionales. |
| /xl | Excluye los archivos y directorios "no hay nada". |
| /is | Incluye los mismos archivos. |
| /It | Incluye archivos modificados. |
| /max:`<n>` | Especifica el tamaño máximo de archivo (para excluir archivos de más de *n* bytes). |
| minuto`<n>` | Especifica el tamaño mínimo de archivo (para excluir archivos de menos de *n* bytes). |
| maxage`<n>` | Especifica la duración máxima del archivo (para excluir archivos de más de *n* días o fecha). |
| /minage:`<n>` | Especifica la antigüedad mínima del archivo (excluir archivos más recientes de *n* días o fecha). |
| /maxlad:`<n>` | Especifica la última fecha de acceso máxima (excluye los archivos no usados desde *n*). |
| /minlad:`<n>` | Especifica la última fecha de acceso mínima (excluye los archivos usados desde *n*) si *n* es menor que 1900, *n* especifica el número de días. De lo contrario, *n* especifica una fecha con el formato AAAAMMDD. |
| /xj | Excluye los puntos de Unión, que normalmente se incluyen de forma predeterminada. |
| /fft | Se da por supuesto que se trata de tiempos de archivos FAT (precisión de dos segundos). |
| /DST | Compensa las diferencias de hora de horario de verano de una hora. |
| /xjd | Excluye los puntos de unión de los directorios. |
| /xjf | Excluye los puntos de Unión para los archivos. |

#### <a name="retry-options"></a>Opciones de reintento

| Opción | Descripción |
|--|--|
| /r:`<n>` | Especifica el número de reintentos en las copias con errores. El valor predeterminado de *n* es 1 millón (1 millón reintentos). |
| /w:`<n>` | Especifica el tiempo de espera entre reintentos, en segundos. El valor predeterminado de *n* es 30 (tiempo de espera de 30 segundos). |
| /reg | Guarda los valores especificados en las opciones **/r** y **/w** como valores predeterminados en el registro. |
| /tbd | Especifica que el sistema esperará a que se definan los nombres de los recursos compartidos (error de reintento 67). |

#### <a name="logging-options"></a>Opciones de registro

| Opción | Descripción |
|--|--|
| /l | Especifica que los archivos solo se mostrarán (y no se copiarán, se eliminarán ni se marcarán en el tiempo). |
| /x | Informa de todos los archivos adicionales, no solo de los que están seleccionados. |
| /v | Genera una salida detallada y muestra todos los archivos omitidos. |
| /TS | Incluye marcas de tiempo de archivo de origen en el resultado. |
| /fp | Incluye los nombres de ruta de acceso completa de los archivos en la salida. |
| /bytes | Imprime los tamaños, como bytes. |
| /NS | Especifica que no se van a registrar los tamaños de archivo. |
| /nc | Especifica que no se registrarán las clases de archivo. |
| /nfl | Especifica que los nombres de archivo no se van a registrar. |
| /ndl | Especifica que los nombres de directorio no se van a registrar. |
| /np | Especifica que no se mostrará el progreso de la operación de copia (el número de archivos o directorios copiados hasta el momento). |
| /eta | Muestra el tiempo estimado de llegada (ETA) de los archivos copiados. |
| /log`<logfile>` | Escribe la salida del estado en el archivo de registro (sobrescribe el archivo de registro existente). |
| /log +:`<logfile>` | Escribe la salida de estado en el archivo de registro (anexa la salida al archivo de registro existente). |
| /unicode | Muestra la salida de estado como texto Unicode. |
| /unilog:`<logfile>` | Escribe la salida de estado en el archivo de registro como texto Unicode (sobrescribe el archivo de registro existente). |
| /UNILOG +:`<logfile>` | Escribe la salida de estado en el archivo de registro como texto Unicode (anexa la salida al archivo de registro existente). |
| /tee | Escribe la salida de estado en la ventana de la consola, así como en el archivo de registro. |
| /njh | Especifica que no hay ningún encabezado de trabajo. |
| /njs | Especifica que no hay ningún Resumen del trabajo. |

#### <a name="job-options"></a>Opciones de trabajo

| Opción | Descripción |
|--|--|
| /trabajo`<jobname>` | Especifica que los parámetros se van a derivar del archivo de trabajo con nombre. |
| /Save`<jobname>` | Especifica que los parámetros se van a guardar en el archivo de trabajo con nombre. |
| /quit | Sale después del procesamiento de la línea de comandos (para ver los parámetros). |
| /nosd | Indica que no se ha especificado ningún directorio de origen. |
| /nodd | Indica que no se ha especificado ningún directorio de destino. |
| /If | Incluye los archivos especificados. |

### <a name="exit-return-codes"></a>Códigos de salida (Return)

| Valor | Descripción |
|--|--|
| 0 | No se copió ningún archivo. No se encontró ningún error. No hubo coincidencia de archivos. Los archivos ya existen en el directorio de destino; por lo tanto, se omitió la operación de copia. |
| 1 | Todos los archivos se han copiado correctamente. |
| 2 | Hay algunos archivos adicionales en el directorio de destino que no están presentes en el directorio de origen. No se copió ningún archivo. |
| 3 | Algunos archivos se han copiado. Existen archivos adicionales. No se encontró ningún error. |
| 5 | Algunos archivos se han copiado. Algunos archivos no coincidían. No se encontró ningún error. |
| 6 | Existen archivos adicionales y archivos no coincidentes. No se copió ningún archivo y no se encontró ningún error. Esto significa que los archivos ya existen en el directorio de destino. |
| 7 | Los archivos se han copiado, se ha producido un error de coincidencia de archivos y existen archivos adicionales. |
| 8 | No se copiaron varios archivos. |

> [!NOTE]
> Cualquier valor mayor que **8** indica que se produjo al menos un error durante la operación de copia.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
