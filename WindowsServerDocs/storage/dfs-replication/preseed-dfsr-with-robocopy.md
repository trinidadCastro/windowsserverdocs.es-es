---
title: Utilizar Robocopy para preseed los archivos para la replicación DFS
description: Cómo usar Robocopy.exe para preseed los archivos para la replicación DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082671"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Utilizar Robocopy para preseed los archivos para la replicación DFS

>Se aplica a: WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, Windows Server2008

En este tema se explica cómo usar la herramienta de línea de comandos, **Robocopy.exe**, para preseed archivos al configurar la replicación para la replicación del sistema de archivos distribuido (DFS) (también conocido como DFSR o DFS-R) en Windows Server. Por preseeding los archivos antes de configurar la replicación DFS, agregue a un nuevo asociado de replicación, o reemplazar un servidor, puede acelerar la sincronización inicial y habilitar la clonación de la base de datos de replicación DFS en Windows Server 2012 R2. El método Robocopy es uno de los diversos métodos preseeding; Para obtener información general, vea [paso 1: preseed archivos para la replicación DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

La utilidad de línea de comandos de Robocopy (copia de archivo sólida) se incluye con Windows Server. La utilidad proporciona muchas opciones que incluyen copia seguridad, copia de seguridad API soporte, capacidades de reintento y registro. Las versiones posteriores incluyen compatibilidad de subprocesamiento múltiple y reactivar almacenados en el búfer de E/S.

>[!IMPORTANT]
>Robocopy no copia los archivos bloqueados exclusivamente. Si los usuarios tienden a bloquear muchos archivos durante largos períodos en los servidores de archivos, considere el uso de un método preseeding diferente. Preseeding no requiere a una coincidencia perfecta entre las listas de archivos en los servidores de origen y de destino, pero es el número de archivos que no existen cuando se realiza la sincronización inicial para la replicación DFS, el preseeding menos eficaz. Para minimizar los conflictos de bloqueo, utilice Robocopy durante las horas para su organización. Siempre se examinan los registros de Robocopy después de preseeding para asegurarse de que comprende qué archivos se han omitido debido a los bloqueos exclusivos.

Para utilizar Robocopy para preseed los archivos para la replicación DFS, siga estos pasos:

1. [Descargue e instale la versión más reciente de Robocopy.](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [Estabilizar los archivos que se van a replicar.](#step-2:-stabilize-files-that-will-be-replicated)
3. [Copie los archivos duplicados en el servidor de destino.](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Requisitos previos

Debido a que preseeding no directamente relacionadas con la replicación DFS, sólo necesita cumplir los requisitos para realizar una copia del archivo con Robocopy.

- Necesita una cuenta que sea miembro del grupo Administradores local en los servidores de origen y de destino.

- Instalar la versión más reciente de Robocopy en el servidor que va a usar para copiar los archivos, el servidor de origen o el servidor de destino; debe instalar la versión más reciente para la versión del sistema operativo. Para obtener instrucciones, vea [paso 2: estabilizar los archivos que se van a replicar](#step-2:-stabilize-files-that-will-be-replicated). A menos que se preseeding los archivos de un servidor que ejecuta Windows Server 2003 R2, puede ejecutar Robocopy en servidor de origen o de destino. El servidor de destino, que normalmente tiene la versión más reciente del sistema operativo, le proporciona acceso a la versión más reciente de Robocopy.

- Asegúrese de que hay suficiente espacio de almacenamiento en la unidad de destino. No cree una carpeta en la ruta de acceso que se va a copiar en: Robocopy debe crear la carpeta raíz.
    
    >[!NOTE]
    >Cuando decida cuánto espacio se debe asignar para los archivos preseeded, considere la posibilidad de crecimiento esperado de los datos a través de los requisitos de almacenamiento y tiempo para la replicación DFS. Para planear la Ayuda, vea [Editar el tamaño de la cuota de la carpeta de almacenamiento provisional y el conflicto y la carpeta eliminados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) en la [Administración de replicación de DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- En el servidor de origen, de forma opcional instalar Monitor de proceso o el Explorador de proceso, que puede usar para comprobar para las aplicaciones que bloquean archivos. Para obtener información de la descarga, vea [Monitor de procesos](https://docs.microsoft.com/sysinternals/downloads/procmon) y [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Paso 1: Descargar e instalar la versión más reciente de Robocopy

Antes de utilizar Robocopy para preseed archivos, debe descargar e instalar la versión más reciente de **Robocopy.exe**. Esto garantiza que la replicación DFS no omitir archivos debido a problemas de versiones de Robocopy trasvase de registros.

El origen de la última versión compatible de Robocopy depende de la versión de Windows Server que se está ejecutando en el servidor. Para obtener información acerca de cómo descargar la revisión con la versión más reciente de Robocopy para Windows Server 2008 R2 o Windows Server 2008, vea lista de revisiones disponibles actualmente para tecnologías de sistema de archivos distribuido (DFS) en Windows Server 2008 y en [ Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Como alternativa, puede buscar e instalar la revisión más reciente para un sistema operativo siguiendo los pasos siguientes.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Buscar e instalar la versión más reciente de Robocopy para una versión específica de Windows Server

1. En un explorador web, abra [https://support.microsoft.com](https://support.microsoft.com/).

2. En **Soporte técnico de búsqueda**, especifique la siguiente cadena, reemplazando `<operating system version>` con el sistema operativo adecuado, a continuación, presione la tecla ENTRAR:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Por ejemplo, escriba **robocopy.exe kbqfe "Windows Server 2008 R2"**.

3. Busque y descargue la revisión con el mayor número de identificador (es decir, la versión más reciente).

4. Instalar la revisión en el servidor.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Paso 2: Estabilizar los archivos que se van a replicar

Después de instalar la versión más reciente de Robocopy en el servidor, debe evitar que archivos bloqueados copien bloqueo mediante el uso de los métodos descritos en la siguiente tabla. No bloquear la mayoría de las aplicaciones archivos exclusivamente. Sin embargo, durante las operaciones normales, un pequeño porcentaje de archivos puede estar bloqueado en servidores de archivos.

|Origen del bloqueo|Explicación|Mitigación|
|---|---|---|
|Forma remota, los usuarios abrir archivos en recursos compartidos.|Los empleados conectan a un servidor de archivos estándar y edición documentos, el contenido multimedia u otros archivos. Conoce a veces como la carpeta principal tradicional o cargas de trabajo de datos compartidos.|Sólo realizar operaciones de Robocopy durante períodos de menos actividad, las horas de inactividad. Esto minimiza el número de archivos que se debe omitir Robocopy durante preseeding.<br><br>Considere la posibilidad de establecer temporalmente acceso de solo lectura en los recursos compartidos de archivos que se van a replicar mediante el uso de los cmdlets de Windows PowerShell **Grant-SmbShareAccess** y **Cerrar SmbSession** . Si establece los permisos para un grupo común como usuarios autenticados o todos los usuarios para lectura, los usuarios estándar es posible que es menos probable que abrir archivos con bloqueos exclusivos (si sus aplicaciones de detectan el acceso de solo lectura cuando se abren los archivos).<br><br>También se puede establecer una regla de firewall temporal para el puerto SMB 445 entrantes a ese servidor para bloquear el acceso a los archivos o use el cmdlet de **Bloque SmbShareAccess** . Sin embargo, ambos de estos métodos son muy perjudiciales a las operaciones de usuario.|
|Aplicaciones abren archivos locales.|Las cargas de trabajo de la aplicación que se ejecuta en un servidor de archivos a veces bloquean archivos.|Deshabilitar temporalmente o desinstalar las aplicaciones que están bloqueando los archivos. Puede usar Monitor de proceso o el Explorador de proceso para determinar las aplicaciones que bloquean archivos. Para descargar el Monitor de proceso o Process Explorer, visite las páginas del [Monitor de procesos](https://docs.microsoft.com/sysinternals/downloads/procmon) y [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) .|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Paso 3: Copie los archivos replicados en el servidor de destino

Después de minimizar los bloqueos en los archivos que se van a replicar, puede preseed los archivos desde el servidor de origen al servidor de destino.

>[!NOTE]
>Puede ejecutar Robocopy en el equipo de origen o el equipo de destino. El siguiente procedimiento describe cómo ejecutar Robocopy en el servidor de destino, que normalmente se ejecuta un sistema operativo más reciente, para aprovechar las funciones Robocopy adicionales que puede proporcionar el sistema operativo más reciente.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Preseed los archivos replicados en el servidor de destino con Robocopy

1. Inicie sesión en el servidor de destino con una cuenta que sea miembro del grupo Administradores local en los servidores de origen y de destino.

2. Abre un símbolo del sistema con privilegios elevados.

3. Para preseed los archivos desde el origen al servidor de destino, ejecute el siguiente comando, sustituyendo su propio origen, destino y rutas de acceso de archivo de registro para los valores entre corchetes:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Este comando copia todo el contenido de la carpeta de origen a la carpeta de destino, con los siguientes parámetros:
    
    |Parámetro|Descripción|
    |---|---|
    |"\ < carpeta replicada ruta de origen >"|Especifica la carpeta de origen que se va a preseed en el servidor de destino.|
    |"\ < destino replicado ruta de carpeta >"|Especifica la ruta de acceso a la carpeta que se va a almacenar los archivos de preseeded.<br><br>La carpeta de destino no debe existir ya en el servidor de destino. Para obtener los hash de archivo coincidente, Robocopy debe crear la carpeta raíz cuando lo preseeds los archivos.|
    |/e|Copia los subdirectorios y sus archivos, así como los subdirectorios vacíos.|
    |/b|Copia los archivos en el modo de copia de seguridad.|
    |/ copyal|Copia todos los información de archivo, incluidos los datos, los atributos, las marcas de tiempo, la lista de control de acceso (ACL) de NTFS, información de propietario e información de auditoría.|
    |/r:6|Reintenta la operación seis veces cuando se produce un error.|
    |/w:5|Espera 5 segundos entre reintentos.|
    |MT:64|Copia los archivos de 64 simultáneamente.|
    |/xd DfsrPrivate|Excluye de la carpeta DfsrPrivate.|
    |/tee|Escribe la salida de estado a la ventana de la consola, así como para el archivo de registro.|
    |/ log \ < ruta de acceso de archivo de registro >|Especifica el archivo de registro para escribir. Sobrescribe el contenido del archivo existente. (Para anexar las entradas en el archivo de registro existente, use `/log+ <log file path>`.)|
    |/v|Produce resultados detallados que incluyen pasan por alto los archivos.|
    
    Por ejemplo, el siguiente comando replica los archivos de la carpeta de origen se replican, E:\\RF01, en la unidad de datos D en el servidor de destino:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Se recomienda que use los parámetros descritos arriba cuando utilice Robocopy para preseed los archivos para la replicación DFS. Sin embargo, puede cambiar algunas de sus valores o agregar parámetros adicionales. Por ejemplo, es posible que descubra a través de las pruebas que tienen la capacidad para establecer un valor más alto (número de subprocesos) para el *parámetro/MT* . Además, si principalmente aquí replicar archivos más grandes, es posible que pueda aumentar el rendimiento de la copia mediante la adición de la opción **/j** para E/S sin búfer. Para obtener más información acerca de los parámetros de Robocopy, consulte la referencia de línea de comandos de [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) .

    >[!WARNING]
    >Para evitar posibles pérdidas de datos al utilizar Robocopy para preseed los archivos para la replicación DFS, no realice los siguientes cambios en los parámetros recomendados:
    >- No use el parámetro */mir* (que refleja un árbol de Active directory) o el parámetro */mov* (que mueve los archivos y, a continuación, elimina el origen).
    >-  No quite las opciones **/e**, **/b**y **/copyall** .

4. Una vez completada la copia, examine el registro de errores o los archivos omitidos. Utilizar Robocopy para copiar todos los archivos omitidos individualmente en lugar de volver a copiar el conjunto completo de archivos. Si se omiten los archivos debido a los bloqueos exclusivos, intente copiar los archivos individuales con Robocopy más tarde o acepte que dichos archivos requieren replicación sobre el cable por la replicación DFS durante la sincronización inicial.

## <a name="next-step"></a>Paso siguiente

Después de completar la copia inicial y utilizar Robocopy para resolver los problemas con los archivos omitidos tantos como sea posible, va a usar el cmdlet **Get-DfsrFileHash** en Windows PowerShell o el comando **Dfsrdiag** para validar los archivos de preseeded mediante la comparación de hash de archivo en los servidores de origen y de destino. Para obtener instrucciones detalladas, consulte [paso 2: validar archivos Preseeded para la replicación DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).