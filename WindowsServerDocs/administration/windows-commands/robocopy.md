---
title: robocopy
description: Obtenga información sobre cómo usar el comando robocopy en Windows y Windows Server para copiar archivos
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c6e8e9-fcb3-4a4a-9d04-2d8c367b6354
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 07/25/2018
ms.openlocfilehash: 7ab2eff32b105916d979a954275e9c9122a06903
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441726"
---
# <a name="robocopy"></a>robocopy

Copia datos de archivo.

## <a name="syntax"></a>Sintaxis

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

## <a name="parameters"></a>Parámetros

|   Parámetro    |                                                                                            Descripción                                                                                             |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<origen >    |                                                                            Especifica la ruta de acceso al directorio de origen.                                                                             |
| \<Destino > |                                                                          Especifica la ruta de acceso al directorio de destino.                                                                          |
|    \<File>     | Especifica el archivo o archivos que se van a copiar. Puede usar caracteres comodín ( **&#42;** o **?** ), si desea. Si el **archivo** no se especifica el parámetro, **\*.\\** \* se utiliza como el valor predeterminado. |
|   \<Opciones >   |                                                                    Especifica las opciones que se usará con el **robocopy** comando.                                                                     |

### <a name="copy-options"></a>Opciones de copia

|Opción|Descripción|
|------|-----------|
|/s|Subdirectorios de copias. Tenga en cuenta que esta opción excluye directorios vacíos.|
|/e|Subdirectorios de copias. Tenga en cuenta que esta opción incluye directorios vacíos. Para obtener más información, consulte [comentarios](#remarks).|
|/lev:\<N>|Copia únicamente la parte superior *N* niveles del árbol de directorio de origen.|
|/z|Copia los archivos en modo reiniciable.|
|/b|Copia los archivos en modo de copia de seguridad.|
|/zb|Utiliza el modo reiniciable. Si se deniega el acceso, esta opción utiliza el modo de copia de seguridad.|
|/efsraw|Copia todos los archivos cifrados en el modo RAW de EFS.|
|/copy:\<CopyFlags>|Especifica las propiedades de archivo que va a copiar. Los siguientes son los valores válidos para esta opción:</br>**D.** datos</br>**Un** atributos</br>**T** marcas de tiempo</br>**S** lista de control de acceso (ACL) de NTFS</br>**O** información del propietario</br>**U** información de auditoría</br>El valor predeterminado de **marcadores** es **DAT** (datos, atributos y las marcas de tiempo).|
|/dcopy:\<copyflags\>|Define lo que desea copiar los directorios. El valor predeterminado es DA. Las opciones son D = datos, A = atributos y T = marcas de tiempo.|
|/sec|Copia los archivos con seguridad (equivalente a **/Copy: DATs**).|
|/COPYALL|Copia toda la información de archivo (equivalente a **DATSOU**).|
|/NOCOPY|No copia ninguna información de archivo (útil con **/purgar**).|
|/secfix|Correcciones de seguridad de archivos de todos los archivos, incluso las omite.|
|/timfix|Correcciones de la hora de todos los archivos, incluso las omite.|
|/Purge|Elimina los archivos de destino y los directorios que ya no existen en el origen. Para obtener más información, consulte [comentarios](#remarks).|
|/mir|Refleja un árbol de directorios (equivalente a **/e** plus **/purgar**). Para obtener más información, consulte [comentarios](#remarks).|
|/mov|Mueve los archivos y los elimina del origen una vez que se copian.|
|/Move|Mueve los archivos y directorios y los elimina del origen una vez que se copian.|
|/a+:[RASHCNET]|Agrega los atributos especificados a los archivos copiados.|
|/a-:[RASHCNET]|Quita los atributos especificados de los archivos copiados.|
|/create|Crea un árbol de directorio y sólo los archivos de longitud cero.|
|/ fat|Crea archivos de destino usando sólo los nombres de longitud de caracteres de archivos FAT 8.3.|
|/256|Desactiva la compatibilidad con rutas de acceso muy largos (más de 256 caracteres).|
|/ mon:\<N >|Supervisa el origen y se vuelve a ejecutar cuando más de *N* se detectan cambios.|
|/MOT:\<M >|Supervisa el origen y se ejecuta de nuevo en *M* minutos si se detectan cambios.|
|/MT[:N]|Crea copias multiproceso con *N* subprocesos. *N* debe ser un entero entre 1 y 128. El valor predeterminado de *N* es 8.</br>El **/MT** parámetro no se puede usar con el **/IPG** y **/EFSRAW** parámetros.</br>Redirigir el resultado mediante **/LOG** opción para mejorar el rendimiento.</br>Nota: El parámetro/MT se aplica a Windows Server 2008 R2 y Windows 7.|
|/rh:hhmm-hhmm|Especifica los tiempos de ejecución cuando es posible que se puede iniciar nuevas copias.|
|/pf|Ejecutar las comprobaciones veces en una base por archivo (no por los pasos).|
|/ipg:n|Especifica el intervalo entre paquetes para liberar ancho de banda en líneas lentas.|
|/sl|No siga los vínculos simbólicos y en su lugar, cree una copia del vínculo.|

> [!IMPORTANT]
> Cuando se usa el **/SECFIX** la opción de copia, especifique el tipo de información de seguridad que desea copiar también con una de estas opciones de copia adicional:
>- **/COPYALL**
>- **/COPY:O**
>- **/COPY:S**
>- **/COPY:U**
>- **/SEC**

### <a name="file-selection-options"></a>Opciones de selección de archivo

|Opción|Descripción|
|------|-----------|
|/a|Copia solo archivos para el que el **archivo** está establecido.|
|/m|Copia solo archivos para el que el **archivo** atributo está establecido y restablece el **archivo** atributo.|
|/ia:[RASHCNETO]|Incluye solo los archivos para el que se establezca cualquiera de los atributos especificados.|
|/xa:[RASHCNETO]|Excluye los archivos para el que se establezca cualquiera de los atributos especificados.|
|/xf \<FileName>[ ...]|Excluye los archivos que coinciden con los nombres especificados o rutas de acceso. Tenga en cuenta que *FileName* puede incluir caracteres comodín ( **&#42;** y **?** ).|
|/xd \<Directory>[ ...]|Excluye los directorios que coinciden con los nombres especificados y las rutas de acceso.|
|/xc|Excluye los archivos modificados.|
|/xn|Excluye los archivos más recientes.|
|/xo|Excluye los archivos antiguos.|
|/xx|Excluye los directorios y archivos adicionales.|
|/xl|Excluye los directorios y archivos "nada".|
|/is|Incluye los mismos archivos.|
|/it|Incluye archivos "ajustado".|
|/max:\<N>|Especifica el tamaño máximo de archivo (para excluir archivos mayores que *N* bytes).|
|/min:\<N >|Especifica el tamaño mínimo de archivo (para excluir archivos menores de *N* bytes).|
|/MaxAge:\<N >|Especifica el tiempo máximo de archivo (para excluir archivos anteriores a *N* días o fecha).|
|/MINAGE:\<N >|Especifica la antigüedad mínima de archivo (excluir archivos más reciente que *N* días o fecha).|
|/maxlad:\<N>|Especifica el número máximo de fecha del último acceso (excluye los archivos no usados desde *N*).|
|/MINLAD:\<N >|Especifica el número mínimo de fecha del último acceso (excluye los archivos que se utilizan desde *N*) si *N* es inferior a 1900, *N* especifica el número de días. En caso contrario, *N* especifica una fecha en el formato AAAAMMDD.|
|/xj|Excluye los puntos de unión, que normalmente se incluyen de forma predeterminada.|
|/fft|Se da por supuesto de archivos FAT tiempos (precisión de dos segundos).|
|/dst|Permite compensar las diferencias de tiempo de horario de verano de una hora.|
|/xjd|Excluye los puntos de unión para directorios.|
|/xjf|Excluye los puntos de unión para los archivos.|

### <a name="retry-options"></a>Las opciones de reintento

|Opción|Descripción|
|------|-----------|
|/ r:\<N >|Especifica el número de reintentos de errores de copia. El valor predeterminado de *N* es 1.000.000 (un millón de reintentos).|
|/w:\<N>|Especifica el tiempo de espera entre reintentos, en segundos. El valor predeterminado de *N* es 30 (30 segundos de tiempo de espera).|
|/reg|Guarda los valores especificados en el **/r** y **/w** opciones como la configuración predeterminada en el registro.|
|/tbd|Especifica que el sistema esperará para que nombres de recurso compartido que se defina (Reintentar error 67).|

### <a name="logging-options"></a>Opciones de registro

|Opción|Descripción|
|------|-----------|
|/l|Especifica que se muestran solo los archivos (y no copiar, eliminar, o con marca de tiempo).|
|/x|Notifica todos los archivos adicionales, no solo los seleccionados.|
|/v|El resultado es detallado y muestra todos los archivos omitidos.|
|/ts|Incluye las marcas de tiempo del archivo de origen en la salida.|
|/fp|Incluye los nombres de ruta de acceso completa de los archivos en la salida.|
|/bytes|Imprime los tamaños, como bytes.|
|/ns|Especifica que los tamaños de archivo no se ha iniciado.|
|/nc|Especifica que las clases de archivo no se ha iniciado.|
|/nfl|Especifica que los nombres de archivo no se ha iniciado.|
|/ndl|Especifica que los nombres de directorio no se ha iniciado.|
|/np|Especifica que no se mostrará el progreso de la operación de copia (el número de archivos o directorios copiados hasta el momento).|
|/eta|Muestra el tiempo estimado de llegada (ETA) de los archivos copiados.|
|/log:\<LogFile>|Escribe la salida del estado en el archivo de registro (sobrescribe el archivo de registro existente).|
|/log+:\<LogFile>|Escribe la salida del estado en el archivo de registro (anexa la salida al archivo de registro existente).|
|/Unicode|Muestra la salida del estado como texto Unicode.|
|/unilog:\<LogFile>|Escribe el estado en el archivo de registro de salida como texto Unicode (sobrescribe el archivo de registro existente).|
|/unilog+:\<LogFile>|Escribe el estado en el archivo de registro de salida como texto Unicode (anexa la salida al archivo de registro existente).|
|/tee|Escribe la salida del estado de la ventana de consola, así como el archivo de registro.|
|/njh|Especifica que no hay ningún encabezado de trabajo.|
|/njs|Especifica que no hay ningún resumen del trabajo.|

### <a name="job-options"></a>Opciones de trabajo

|Opción|Descripción|
|------|-----------|
|/job:\<JobName>|Especifica que los parámetros se va a derivar desde el archivo de trabajo con nombre.|
|/ Guardar:\<JobName >|Especifica que los parámetros se guardan en el archivo de trabajo con nombre.|
|/quit|Se cierra después de la línea de comandos de procesamiento (para ver los parámetros).|
|/nosd|Indica que no se ha especificado ningún directorio de origen.|
|/nodd|Indica que no se ha especificado ningún directorio de destino.|
|/if|Incluye los archivos especificados.|

### <a name="exit-return-codes"></a>Códigos de salida (devolución)

Valor | Descripción
-- | --
0 | Se copia ningún archivo. No se detectó ningún error.  No hay archivos no coinciden. Los archivos ya existen en el directorio de destino. por lo tanto, se omitió la operación de copia.
1 | Todos los archivos se copiaron correctamente.
2 | Hay algunos archivos adicionales en el directorio de destino que no están presentes en el directorio de origen. Se copia ningún archivo.
3 | Algunos archivos se han copiado. Archivos adicionales estaban presentes. No se detectó ningún error.
5 | Algunos archivos se han copiado. Algunos archivos no coinciden. No se detectó ningún error.
6 | Existen archivos adicionales y los archivos que no coincidentes. Se copia ningún archivo y no hay errores encontrados. Esto significa que los archivos ya existen en el directorio de destino.
7 | Se copiaron los archivos, estaba presente un error de coincidencia de archivos y archivos adicionales que estaban presentes.
8 | No se copió varios archivos.

> [!NOTE]
> Cualquier valor mayor que 8 indica que se produjo al menos un error durante la operación de copia.

### <a name="remarks"></a>Comentarios

-   El **/mir** opción es equivalente a la **/e** plus **/purgar** opciones con una pequeña diferencia de comportamiento:  
    -   Con el **/e** plus **/purgar** opciones, si existe el directorio de destino, la configuración de seguridad del directorio de destino no se sobrescriben.
    -   Con el **/mir** opción, si existe el directorio de destino, la configuración de seguridad del directorio de destino se sobrescriben.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
