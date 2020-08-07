---
title: robocopy
description: Obtenga información acerca de cómo usar el comando Robocopy en Windows y Windows Server para copiar archivos
ms.topic: article
ms.assetid: d4c6e8e9-fcb3-4a4a-9d04-2d8c367b6354
author: jasongerend
ms.author: jgerend
manager: lizapo
ms.date: 06/07/2020
ms.openlocfilehash: fdf7eda5a17dccba0f43cca91cae122872dd5235
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883491"
---
# <a name="robocopy"></a>robocopy

Copia los datos de archivo.

## <a name="syntax"></a>Sintaxis

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

Por ejemplo, para copiar un archivo llamado *yearly-Report. mov* de c:\Informes a un recurso compartido de archivos (* \\ marketing\videos*) mientras se habilita el multithreading para obtener un mayor rendimiento (con el parámetro/MT) y la capacidad de reiniciar la transferencia en caso de que se interrumpa (con el parámetro/z), usaría la siguiente sintaxis:

```dos
robocopy C:\reports '\\marketing\videos' yearly-report.mov /mt /z
```

### <a name="parameters"></a>Parámetros

|   Parámetro    |                                                                                            Descripción                                                                                           |
|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<Source>    |                                                                            Especifica la ruta de acceso del directorio de origen.                                                                           |
| \<Destination> |                                                                          Especifica la ruta de acceso del directorio de destino.                                                                        |
|    \<File>     | Especifica el archivo o los archivos que se van a copiar. Si lo desea, puede usar caracteres comodín (**&#42;** o **?**). Si no se especifica el parámetro **File** , ** \* \* se** usa como valor predeterminado. |
|   \<Options>   |                                                                    Especifica las opciones que se van a usar con el comando **Robocopy** .                                                                   |

### <a name="copy-options"></a>Opciones de copia

|Opción|Descripción|
|------|-----------|
|/s|Copia subdirectorios. Tenga en cuenta que esta opción excluye los directorios vacíos.|
|/e|Copia subdirectorios. Tenga en cuenta que esta opción incluye directorios vacíos. Para obtener más información, vea la [sección Comentarios](#remarks).|
|nivel\<N>|Copia solo los *N* niveles principales del árbol de directorio de origen.|
|/z|Copia los archivos en modo reiniciable.|
|/b|Copia los archivos en modo de copia de seguridad.|
|/zb|Utiliza el modo reiniciable. Si se deniega el acceso, esta opción utiliza el modo de copia de seguridad.|
|/efsraw|Copia todos los archivos cifrados en modo RAW de EFS.|
|/Copy\<CopyFlags>|Especifica las propiedades de archivo que se van a copiar. Estos son los valores válidos para esta opción:</br>Datos **D**</br>**Atributos**</br>Marcas de tiempo de **T**</br>**S** lista de control de acceso (ACL) de NTFS</br>**O** información de propietario</br>**U** información de auditoría</br>El valor predeterminado de **CopyFlags** es **DAT** (datos, atributos y marcas de tiempo).|
|/dcopy:\<copyflags\>|Define qué copiar para los directorios. El valor predeterminado es DA. Las opciones son D = Data, A = Attributes y T = timestamps.|
|/s|Copia los archivos con seguridad (equivalente a **/Copy: DATS**).|
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
|/create|Solo crea un árbol de directorios y archivos de longitud cero.|
|/fat|Crea archivos de destino usando únicamente nombres de archivo FAT de longitud de caracteres 8,3.|
|/256|Desactiva la compatibilidad con rutas de acceso muy largas (más de 256 caracteres).|
|mes\<N>|Supervisa el origen y vuelve a ejecutarse cuando se detectan más de *N* cambios.|
|/mot:\<M>|Supervisa el origen y vuelve a ejecutarse en *M* minutos si se detectan cambios.|
|/MT[:N]|Crea copias multiproceso con *N* subprocesos. *N* debe ser un entero comprendido entre 1 y 128. El valor predeterminado de *N* es 8.</br>El parámetro **/MT** no se puede usar con los parámetros **/IPG** y **/EFSRAW** .</br>Redirija la salida mediante la opción **/log** para obtener un mejor rendimiento.</br>Nota: el parámetro/MT se aplica a Windows Server 2008 R2 y Windows 7.|
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
|/XF \<FileName> [...]|Excluye los archivos que coinciden con los nombres o rutas de acceso especificados. Tenga en cuenta que *filename* puede incluir caracteres comodín (**&#42;** y **?**).|
|/XD \<Directory> [...]|Excluye los directorios que coinciden con los nombres y rutas de acceso especificados.|
|/xc|Excluye los archivos modificados.|
|/xn|Excluye los archivos más recientes.|
|/xo|Excluye los archivos más antiguos.|
|/xx|Excluye archivos y directorios adicionales.|
|/xl|Excluye los archivos y directorios "no hay nada".|
|/is|Incluye los mismos archivos.|
|/It|Incluye archivos "reajustados".|
|/max:\<N>|Especifica el tamaño máximo de archivo (para excluir archivos de más de *N* bytes).|
|minuto\<N>|Especifica el tamaño mínimo de archivo (para excluir archivos de menos de *N* bytes).|
|maxage\<N>|Especifica la duración máxima del archivo (para excluir archivos de más de *N* días o fecha).|
|/minage:\<N>|Especifica la antigüedad mínima del archivo (excluir archivos más recientes de *N* días o fecha).|
|/maxlad:\<N>|Especifica la última fecha de acceso máxima (excluye los archivos no usados desde *N*).|
|/minlad:\<N>|Especifica la última fecha de acceso mínima (excluye los archivos usados desde *n*) si *n* es menor que 1900, *n* especifica el número de días. De lo contrario, *N* especifica una fecha con el formato AAAAMMDD.|
|/xj|Excluye los puntos de Unión, que normalmente se incluyen de forma predeterminada.|
|/fft|Se da por supuesto que se trata de tiempos de archivos FAT (precisión de dos segundos).|
|/DST|Compensa las diferencias de hora de horario de verano de una hora.|
|/xjd|Excluye los puntos de unión de los directorios.|
|/xjf|Excluye los puntos de Unión para los archivos.|

### <a name="retry-options"></a>Opciones de reintento

|Opción|Descripción|
|------|-----------|
|/r\<N>|Especifica el número de reintentos en las copias con errores. El valor predeterminado de *N* es 1 millón (1 millón reintentos).|
|/w\<N>|Especifica el tiempo de espera entre reintentos, en segundos. El valor predeterminado de *N* es 30 (tiempo de espera de 30 segundos).|
|/reg|Guarda los valores especificados en las opciones **/r** y **/w** como valores predeterminados en el registro.|
|/tbd|Especifica que el sistema esperará a que se definan los nombres de los recursos compartidos (error de reintento 67).|

### <a name="logging-options"></a>Opciones de registro

|Opción|Descripción|
|------|-----------|
|/l|Especifica que los archivos solo se mostrarán (y no se copiarán, se eliminarán ni se marcarán en el tiempo).|
|/x|Informa de todos los archivos adicionales, no solo de los que están seleccionados.|
|/v|Genera una salida detallada y muestra todos los archivos omitidos.|
|/TS|Incluye marcas de tiempo de archivo de origen en el resultado.|
|/fp|Incluye los nombres de ruta de acceso completa de los archivos en la salida.|
|/bytes|Imprime los tamaños, como bytes.|
|/NS|Especifica que no se van a registrar los tamaños de archivo.|
|/nc|Especifica que no se registrarán las clases de archivo.|
|/nfl|Especifica que los nombres de archivo no se van a registrar.|
|/ndl|Especifica que los nombres de directorio no se van a registrar.|
|/np|Especifica que no se mostrará el progreso de la operación de copia (el número de archivos o directorios copiados hasta el momento).|
|/eta|Muestra el tiempo estimado de llegada (ETA) de los archivos copiados.|
|/log\<LogFile>|Escribe la salida del estado en el archivo de registro (sobrescribe el archivo de registro existente).|
|/log +:\<LogFile>|Escribe la salida de estado en el archivo de registro (anexa la salida al archivo de registro existente).|
|/unicode|Muestra la salida de estado como texto Unicode.|
|/unilog:\<LogFile>|Escribe la salida de estado en el archivo de registro como texto Unicode (sobrescribe el archivo de registro existente).|
|/UNILOG +:\<LogFile>|Escribe la salida de estado en el archivo de registro como texto Unicode (anexa la salida al archivo de registro existente).|
|/tee|Escribe la salida de estado en la ventana de la consola, así como en el archivo de registro.|
|/njh|Especifica que no hay ningún encabezado de trabajo.|
|/njs|Especifica que no hay ningún Resumen del trabajo.|

### <a name="job-options"></a>Opciones de trabajo

|Opción|Descripción|
|------|-----------|
|/trabajo\<JobName>|Especifica que los parámetros se van a derivar del archivo de trabajo con nombre.|
|/Save\<JobName>|Especifica que los parámetros se van a guardar en el archivo de trabajo con nombre.|
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

### <a name="remarks"></a>Observaciones

-   La opción **/Mir** es equivalente a las opciones **/e** Plus **/Purge** con una pequeña diferencia en el comportamiento:
    -   Con las opciones **/e** Plus **/Purge** , si el directorio de destino existe, no se sobrescribe la configuración de seguridad del directorio de destino.
    -   Con la opción **/Mir** , si existe el directorio de destino, se sobrescribe la configuración de seguridad del directorio de destino.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
