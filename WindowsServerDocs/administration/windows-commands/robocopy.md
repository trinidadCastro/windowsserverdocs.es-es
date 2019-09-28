---
title: robocopy
description: Obtenga información acerca de cómo usar el comando Robocopy en Windows y Windows Server para copiar archivos
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b814134dd8ca82a4338f80aba26c5a7dcee3b90a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384500"
---
# <a name="robocopy"></a>robocopy

Copia los datos de archivo.

## <a name="syntax"></a>Sintaxis

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

## <a name="parameters"></a>Parámetros

|   Parámetro    |                                                                                            Descripción                                                                                             |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   @no__t 0Source >    |                                                                            Especifica la ruta de acceso al directorio de origen.                                                                             |
| @no__t 0Destination > |                                                                          Especifica la ruta de acceso al directorio de destino.                                                                          |
|    @no__t 0File >     | Especifica el archivo o los archivos que se van a copiar. Si lo desea, puede usar **&#42;** caracteres comodín (o **?** ). Si no se especifica el parámetro **File** , **\*. \\** \* se utiliza como valor predeterminado. |
|   @no__t 0Options >   |                                                                    Especifica las opciones que se van a usar con el comando **Robocopy** .                                                                     |

### <a name="copy-options"></a>Opciones de copia

|Opción|Descripción|
|------|-----------|
|/s|Copia subdirectorios. Tenga en cuenta que esta opción excluye los directorios vacíos.|
|/e|Copia subdirectorios. Tenga en cuenta que esta opción incluye directorios vacíos. Para obtener más información, vea la [sección Comentarios](#remarks).|
|/Lev: \<n (>|Copia solo los *N* niveles principales del árbol de directorio de origen.|
|/z|Copia los archivos en modo reiniciable.|
|b|Copia los archivos en modo de copia de seguridad.|
|/zb|Utiliza el modo reiniciable. Si se deniega el acceso, esta opción utiliza el modo de copia de seguridad.|
|/efsraw|Copia todos los archivos cifrados en modo RAW de EFS.|
|/Copy: @no__t 0CopyFlags >|Especifica las propiedades de archivo que se van a copiar. Estos son los valores válidos para esta opción:</br>Datos **D**</br>**Atributos**</br>Marcas de tiempo de **T**</br>**S** lista de control de acceso (ACL) de NTFS</br>**O** información de propietario</br>**U** información de auditoría</br>El valor predeterminado de **CopyFlags** es **DAT** (datos, atributos y marcas de tiempo).|
|/dcopy: \<copyflags @ no__t-1|Define qué copiar para los directorios. El valor predeterminado es DA. Las opciones son D = Data, A = Attributes y T = timestamps.|
|s|Copia los archivos con seguridad (equivalente a **/Copy: DATS**).|
|/copyall|Copia toda la información de archivo (equivalente a **/Copy: DATSOU**).|
|/nocopy|No copia ninguna información de archivo (útil con **/Purge**).|
|/secfix|Corrige la seguridad de los archivos en todos los archivos, incluso los omitidos.|
|/timfix|Corrige los tiempos de archivo en todos los archivos, incluso los omitidos.|
|/purge|Elimina los archivos y directorios de destino que ya no existen en el origen. Para obtener más información, vea la [sección Comentarios](#remarks).|
|/mir|Refleja un árbol de directorios (equivalente a **/e** más **/Purge**). Para obtener más información, vea la [sección Comentarios](#remarks).|
|/mov|Mueve archivos y los elimina del origen una vez copiados.|
|/Move|Mueve archivos y directorios, y los elimina del origen una vez copiados.|
|/a +: [RASHCNET]|Agrega los atributos especificados a los archivos copiados.|
|/a-(: [RASHCNET]|Quita los atributos especificados de los archivos copiados.|
|/Create|Solo crea un árbol de directorios y archivos de longitud cero.|
|/fat|Crea archivos de destino usando únicamente nombres de archivo FAT de longitud de caracteres 8,3.|
|/256|Desactiva la compatibilidad con rutas de acceso muy largas (más de 256 caracteres).|
|/Mon: \<n (>|Supervisa el origen y vuelve a ejecutarse cuando se detectan más de *N* cambios.|
|/MOT: \<M >|Supervisa el origen y vuelve a ejecutarse en *M* minutos si se detectan cambios.|
|/MT [: N]|Crea copias multiproceso con *N* subprocesos. *N* debe ser un entero comprendido entre 1 y 128. El valor predeterminado de *N* es 8.</br>El parámetro **/MT** no se puede usar con los parámetros **/IPG** y **/EFSRAW** .</br>Redirija la salida mediante la opción **/log** para obtener un mejor rendimiento.</br>Nota: El parámetro/MT se aplica a Windows Server 2008 R2 y Windows 7.|
|/RH: hhmm-hhmm|Especifica los tiempos de ejecución cuando se pueden iniciar nuevas copias.|
|/PF|Comprueba los tiempos de ejecución por archivo (no por paso).|
|/IPG: n|Especifica el intervalo entre paquetes para el ancho de banda libre en líneas lentas.|
|/sl|No siga los vínculos simbólicos y, en su lugar, cree una copia del vínculo.|

> [!IMPORTANT]
> Al usar la opción de copia **/SECFIX** , especifique el tipo de información de seguridad que desea copiar también mediante una de estas opciones de copia adicionales:
>- **/COPYALL**
>- **/COPY: O**
>- **/COPY: S**
>- **/COPY: U**
>- **S**

### <a name="file-selection-options"></a>Opciones de selección de archivos

|Opción|Descripción|
|------|-----------|
|/a|Copia solo los archivos para los que se establece el atributo de **archivo** .|
|/m|Copia solo los archivos para los que se establece el atributo **Archive** y restablece el atributo **Archive** .|
|/IA: [RASHCNETO]|Incluye solo los archivos para los que se establece cualquiera de los atributos especificados.|
|/xa:[RASHCNETO]|Excluye los archivos para los que se establece cualquiera de los atributos especificados.|
|/XF \<FileName > [...]|Excluye los archivos que coinciden con los nombres o rutas de acceso especificados. Tenga en cuenta que *filename* puede incluir caracteres **&#42;** comodín (y **?** ).|
|/XD \<Directory > [...]|Excluye los directorios que coinciden con los nombres y rutas de acceso especificados.|
|/xc|Excluye los archivos modificados.|
|/xn|Excluye los archivos más recientes.|
|/xo|Excluye los archivos más antiguos.|
|/xx|Excluye archivos y directorios adicionales.|
|/xl|Excluye los archivos y directorios "no hay nada".|
|/is|Incluye los mismos archivos.|
|/It|Incluye archivos "reajustados".|
|/Max: \<n (>|Especifica el tamaño máximo de archivo (para excluir archivos de más de *N* bytes).|
|/min: \<n (>|Especifica el tamaño mínimo de archivo (para excluir archivos de menos de *N* bytes).|
|/maxage: \<n (>|Especifica la duración máxima del archivo (para excluir archivos de más de *N* días o fecha).|
|/Minage: \<n (>|Especifica la antigüedad mínima del archivo (excluir archivos más recientes de *N* días o fecha).|
|/maxlad: \<n (>|Especifica la última fecha de acceso máxima (excluye los archivos no usados desde *N*).|
|/minlad: \<n (>|Especifica la última fecha de acceso mínima (excluye los archivos usados desde *n*) si *n* es menor que 1900, *n* especifica el número de días. De lo contrario, *N* especifica una fecha con el formato AAAAMMDD.|
|/xj|Excluye los puntos de Unión, que normalmente se incluyen de forma predeterminada.|
|/fft|Supone un tiempo de archivo FAT (precisión de dos segundos).|
|/DST|Compensa las diferencias de hora de horario de verano de una hora.|
|/xjd|Excluye los puntos de unión de los directorios.|
|/xjf|Excluye los puntos de Unión para los archivos.|

### <a name="retry-options"></a>Opciones de reintento

|Opción|Descripción|
|------|-----------|
|/r: @no__t 0n (>|Especifica el número de reintentos en las copias con errores. El valor predeterminado de *N* es 1 millón (1 millón reintentos).|
|/w: @no__t 0n (>|Especifica el tiempo de espera entre reintentos, en segundos. El valor predeterminado de *N* es 30 (tiempo de espera de 30 segundos).|
|/reg|Guarda los valores especificados en las opciones **/r** y **/w** como valores predeterminados en el registro.|
|/tbd|Especifica que el sistema esperará a que se definan los nombres de los recursos compartidos (error de reintento 67).|

### <a name="logging-options"></a>Opciones de registro

|Opción|Descripción|
|------|-----------|
|/l|Especifica que los archivos solo se mostrarán (y no se copiarán, se eliminarán ni se marcarán en el tiempo).|
|/x|Informa de todos los archivos adicionales, no solo de los que están seleccionados.|
|/v|Genera una salida detallada y muestra todos los archivos omitidos.|
|/TS|Incluye marcas de tiempo de archivo de origen en el resultado.|
|/FP|Incluye los nombres de ruta de acceso completa de los archivos en la salida.|
|/bytes|Imprime los tamaños, como bytes.|
|/NS|Especifica que no se van a registrar los tamaños de archivo.|
|/nc|Especifica que no se registrarán las clases de archivo.|
|/nfl|Especifica que no se van a registrar los nombres de archivo.|
|/ndl|Especifica que no se van a registrar los nombres de directorio.|
|/np|Especifica que no se mostrará el progreso de la operación de copia (el número de archivos o directorios copiados hasta el momento).|
|/eta|Muestra el tiempo estimado de llegada (ETA) de los archivos copiados.|
|/log: @no__t 0LogFile >|Escribe la salida de estado en el archivo de registro (sobrescribe el archivo de registro existente).|
|/log +: \<LogFile >|Escribe la salida de estado en el archivo de registro (anexa la salida al archivo de registro existente).|
|/Unicode|Muestra la salida de estado como texto Unicode.|
|/UNILOG: \<LogFile >|Escribe la salida de estado en el archivo de registro como texto Unicode (sobrescribe el archivo de registro existente).|
|/UNILOG +: \<LogFile >|Escribe la salida de estado en el archivo de registro como texto Unicode (anexa la salida al archivo de registro existente).|
|/tee|Escribe la salida de estado en la ventana de la consola, así como en el archivo de registro.|
|/njh|Especifica que no hay ningún encabezado de trabajo.|
|/njs|Especifica que no hay ningún Resumen del trabajo.|

### <a name="job-options"></a>Opciones de trabajo

|Opción|Descripción|
|------|-----------|
|/trabajo: \<JobName >|Especifica que los parámetros se van a derivar del archivo de trabajo con nombre.|
|/Save: @no__t 0JobName >|Especifica que los parámetros se van a guardar en el archivo de trabajo con nombre.|
|/quit|Sale después del procesamiento de la línea de comandos (para ver los parámetros).|
|/nosd|Indica que no se ha especificado ningún directorio de origen.|
|/nodd|Indica que no se ha especificado ningún directorio de destino.|
|/If|Incluye los archivos especificados.|

### <a name="exit-return-codes"></a>Códigos de salida (Return)

Valor | Descripción
-- | --
0 | No se copió ningún archivo. No se encontró ningún error.  No hubo coincidencia de archivos. Los archivos ya existen en el directorio de destino; por lo tanto, se omitió la operación de copia.
1 | Todos los archivos se han copiado correctamente.
2 | Hay algunos archivos adicionales en el directorio de destino que no están presentes en el directorio de origen. No se copió ningún archivo.
3 | Algunos archivos se han copiado. Existen archivos adicionales. No se encontró ningún error.
5 | Algunos archivos se han copiado. Algunos archivos no coincidían. No se encontró ningún error.
6 | Existen archivos adicionales y archivos no coincidentes. No se copió ningún archivo y no se encontró ningún error. Esto significa que los archivos ya existen en el directorio de destino.
7 | Los archivos se han copiado, se ha producido un error de coincidencia de archivos y existen archivos adicionales.
8 | No se copiaron varios archivos.

> [!NOTE]
> Cualquier valor mayor que 8 indica que se produjo al menos un error durante la operación de copia.

### <a name="remarks"></a>Comentarios

-   La opción **/Mir** es equivalente a las opciones **/e** Plus **/Purge** con una pequeña diferencia en el comportamiento:  
    -   Con las opciones **/e** Plus **/Purge** , si el directorio de destino existe, no se sobrescribe la configuración de seguridad del directorio de destino.
    -   Con la opción **/Mir** , si existe el directorio de destino, se sobrescribe la configuración de seguridad del directorio de destino.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
