---
title: Usar Robocopy para preinicializar archivos para Replicación DFS
description: Cómo usar Robocopy. exe para preinicializar archivos para Replicación DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4a0cad3c685c8609784c7096fe31d55294712c2e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871978"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Usar Robocopy para preinicializar archivos para Replicación DFS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

En este tema se explica cómo usar la herramienta de línea de comandos, **Robocopy. exe**, para preinicializar los archivos al configurar la replicación para la replicación sistema de archivos distribuido (DFS) (también conocida como DFSR o DFS-R) en Windows Server. Al preinicializar los archivos antes de configurar Replicación DFS, agregar un nuevo asociado de replicación o reemplazar un servidor, puede acelerar la sincronización inicial y habilitar la clonación de la base de datos Replicación DFS en Windows Server 2012 R2. El método Robocopy es uno de varios métodos de prepropagación. para obtener información general, consulte [Step 1: preseed files for replicación DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

La utilidad de línea de comandos Robocopy (copia de archivos robusta) se incluye con Windows Server. La utilidad proporciona amplias opciones que incluyen la copia de seguridad, la compatibilidad con la API de copia de seguridad, las capacidades de reintento y el registro. Las versiones posteriores incluyen compatibilidad con subprocesamiento múltiple y sin almacenamiento en búfer.

>[!IMPORTANT]
>Robocopy no copia archivos bloqueados exclusivamente. Si los usuarios tienden a bloquear muchos archivos durante largos períodos en los servidores de archivos, considere la posibilidad de usar un método de prepropagación diferente. La inicialización previa no requiere una coincidencia perfecta entre las listas de archivos en los servidores de origen y de destino, pero los más archivos que no existen cuando la sincronización inicial se realiza para Replicación DFS, la inicialización menos efectiva es. Para minimizar los conflictos de bloqueo, use Robocopy durante las horas de poca actividad de la organización. Examine siempre los registros de Robocopy después de la inicialización previa para asegurarse de que comprende qué archivos se omitieron debido a bloqueos exclusivos.

Para usar Robocopy para preinicializar archivos para Replicación DFS, siga estos pasos:

1. [Descargue e instale la versión más reciente de Robocopy.](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [Estabilice los archivos que se van a replicar.](#step-2-stabilize-files-that-will-be-replicated)
3. [Copie los archivos replicados en el servidor de destino.](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Requisitos previos

Dado que la prepropagación no implica directamente Replicación DFS, solo necesita cumplir los requisitos para realizar una copia de archivos con Robocopy.

- Necesita una cuenta que sea miembro del grupo de administradores locales tanto en el servidor de origen como en el de destino.

- Instale la versión más reciente de Robocopy en el servidor que va a usar para copiar los archivos, ya sea el servidor de origen o el servidor de destino. tendrá que instalar la versión más reciente de la versión del sistema operativo. Para obtener instrucciones, [consulte el paso 2: Estabilice los archivos que se van a](#step-2-stabilize-files-that-will-be-replicated)replicar. A menos que esté preinicializando archivos de un servidor que ejecute Windows Server 2003 R2, puede ejecutar Robocopy en el servidor de origen o de destino. El servidor de destino, que normalmente tiene la versión más reciente del sistema operativo, le proporciona acceso a la versión más reciente de Robocopy.

- Asegúrese de que haya suficiente espacio de almacenamiento disponible en la unidad de destino. No cree una carpeta en la ruta de acceso en la que va a copiar: Robocopy debe crear la carpeta raíz.
    
    >[!NOTE]
    >Cuando decida cuánto espacio desea asignar para los archivos preiniciados, tenga en cuenta el crecimiento esperado de los datos en el tiempo y los requisitos de almacenamiento para Replicación DFS. Para planear la ayuda, consulte [editar el tamaño de cuota de la carpeta provisional y la carpeta conflictos y eliminaciones](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) en [Administración de replicación DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- En el servidor de origen, opcionalmente, instale el monitor de procesos o el explorador de procesos, que puede usar para comprobar si hay aplicaciones que bloqueen archivos. Para obtener información sobre la descarga, consulte [monitor de procesos](https://docs.microsoft.com/sysinternals/downloads/procmon) y [Explorador de procesos](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Paso 1: Descargar e instalar la versión más reciente de Robocopy

Antes de usar Robocopy para preinicializar archivos, debe descargar e instalar la versión más reciente de **Robocopy. exe**. Esto garantiza que Replicación DFS no omita archivos debido a problemas en las versiones de envío de Robocopy.

El origen de la última versión de Robocopy compatible depende de la versión de Windows Server que se esté ejecutando en el servidor. Para obtener información acerca de cómo descargar la revisión con la versión más reciente de Robocopy para Windows Server 2008 R2 o Windows Server 2008, consulte la [lista de revisiones disponibles actualmente para las tecnologías de sistema de archivos distribuido (DFS) en Windows server 2008 y en Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Como alternativa, puede buscar e instalar la revisión más reciente para un sistema operativo realizando los pasos siguientes.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Busque e instale la versión más reciente de Robocopy para una versión específica de Windows Server

1. En un explorador Web, Abra [https://support.microsoft.com](https://support.microsoft.com/).

2. En **soporte técnico de búsqueda**, escriba la siguiente cadena `<operating system version>` , reemplazando por el sistema operativo adecuado y, a continuación, presione la tecla entrar:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Por ejemplo, escriba **Robocopy. exe kbqfe "Windows Server 2008 R2"** .

3. Busque y descargue la revisión con el número de identificación más alto (es decir, la versión más reciente).

4. Instale la revisión en el servidor.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Paso 2: Estabilizar los archivos que se van a replicar

Después de instalar la versión más reciente de Robocopy en el servidor, debe evitar que los archivos bloqueados bloqueen la copia con los métodos descritos en la tabla siguiente. La mayoría de las aplicaciones no bloquea exclusivamente los archivos. Sin embargo, durante las operaciones normales, es posible que un pequeño porcentaje de archivos esté bloqueado en los servidores de archivos.

|Origen del bloqueo|Explicación|Solución|
|---|---|---|
|Los usuarios abren archivos en recursos compartidos de forma remota.|Los empleados se conectan a un servidor de archivos estándar y editan documentos, contenido multimedia u otros archivos. A veces se conoce como la carpeta particular tradicional o las cargas de trabajo de datos compartidos.|Realice operaciones de Robocopy solo durante horas de poca actividad y no comerciales. Esto minimiza el número de archivos que Robocopy debe omitir durante la inicialización.<br><br>Considere la posibilidad de configurar temporalmente el acceso de solo lectura en los recursos compartidos de archivos que se van a replicar mediante los cmdlets **Grant-SmbShareAccess** y **Close-SmbSession** de Windows PowerShell. Si establece permisos para un grupo común, como todos o usuarios autenticados que se van a leer, es menos probable que los usuarios estándar abran archivos con bloqueos exclusivos (si sus aplicaciones detectan el acceso de solo lectura cuando se abren archivos).<br><br>También puede considerar la posibilidad de establecer una regla de Firewall temporal para el puerto SMB 445 entrante a ese servidor para bloquear el acceso a los archivos o usar el cmdlet **Block-SmbShareAccess** . Sin embargo, ambos métodos son muy molestos para las operaciones de usuario.|
|Aplicaciones abrir archivos local.|Las cargas de trabajo de aplicaciones que se ejecutan en un servidor de archivos a veces bloquean los archivos.|Deshabilite o desinstale temporalmente las aplicaciones que están bloqueando archivos. Puede usar el monitor de procesos o el explorador de procesos para determinar qué aplicaciones están bloqueando archivos. Para descargar el monitor de procesos o el explorador de procesos, visite las páginas [monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) de procesos y [Explorador de procesos](https://docs.microsoft.com/sysinternals/downloads/process-explorer) .|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Paso 3: Copiar los archivos replicados en el servidor de destino

Después de minimizar los bloqueos en los archivos que se van a replicar, puede preinicializar los archivos del servidor de origen en el servidor de destino.

>[!NOTE]
>Puede ejecutar Robocopy en el equipo de origen o en el equipo de destino. En el procedimiento siguiente se describe la ejecución de Robocopy en el servidor de destino, que normalmente ejecuta un sistema operativo más reciente, para aprovechar las funcionalidades de Robocopy adicionales que pueda proporcionar el sistema operativo más reciente.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Preinicializar los archivos replicados en el servidor de destino con Robocopy

1. Inicie sesión en el servidor de destino con una cuenta que sea miembro del grupo local Administradores en los servidores de origen y de destino.

2. Abre un símbolo del sistema con privilegios elevados.

3. Para preinicializar los archivos del servidor de origen al de destino, ejecute el siguiente comando, sustituyendo sus propias rutas de acceso de origen, destino y archivo de registro por los valores entre corchetes:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Este comando copia todo el contenido de la carpeta de origen en la carpeta de destino, con los parámetros siguientes:
    
    |Parámetro|Descripción|
    |---|---|
    |"\<ruta de acceso\>de la carpeta replicada de origen"|Especifica la carpeta de origen para el preinicialización en el servidor de destino.|
    |"\<ruta\>de la carpeta replicada de destino"|Especifica la ruta de acceso a la carpeta que almacenará los archivos preinicializados.<br><br>La carpeta de destino no debe existir ya en el servidor de destino. Para obtener los hash de archivo coincidentes, Robocopy debe crear la carpeta raíz cuando se preparan los archivos.|
    |/e|Copia subdirectorios y sus archivos, así como subdirectorios vacíos.|
    |b|Copia los archivos en modo de copia de seguridad.|
    |/copyall|Copia toda la información de los archivos, incluidos los datos, los atributos, las marcas de tiempo, la lista de control de acceso (ACL) de NTFS, la información de propietario y la información de auditoría.|
    |/r: 6|Vuelve a intentar la operación seis veces cuando se produce un error.|
    |/w: 5|Espera 5 segundos entre reintentos.|
    |MT: 64|Copia los archivos de 64 simultáneamente.|
    |/XD DfsrPrivate|Excluye la carpeta DfsrPrivate.|
    |/tee|Escribe la salida de estado en la ventana de la consola, así como en el archivo de registro.|
    |Ruta \<del archivo de registro de/log >|Especifica el archivo de registro que se va a escribir. Sobrescribe el contenido existente del archivo. (Para anexar las entradas al archivo de registro existente, `/log+ <log file path>`use).|
    |/v|Genera una salida detallada que incluye archivos omitidos.|
    
    Por ejemplo, el siguiente comando replica los archivos de la carpeta replicada de origen, E\\: RF01, en la unidad de datos D en el servidor de destino:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Se recomienda usar los parámetros descritos anteriormente al usar Robocopy para preinicializar archivos para Replicación DFS. Sin embargo, puede cambiar algunos de sus valores o agregar parámetros adicionales. Por ejemplo, puede que vea las pruebas de que tiene la capacidad de establecer un valor mayor (recuento de subprocesos) para el parámetro */MT* . Además, si va a replicar principalmente archivos de mayor tamaño, es posible que pueda aumentar el rendimiento de las copias agregando la opción **/j** para e/s no almacenadas en búfer. Para obtener más información sobre los parámetros de Robocopy, consulte la referencia de línea de comandos de [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) .

    >[!WARNING]
    >Para evitar la posible pérdida de datos cuando se utiliza Robocopy para preinicializar archivos para Replicación DFS, no realice los cambios siguientes en los parámetros recomendados:
    >- No use el parámetro */Mir* (que refleje un árbol de directorios) o el parámetro */MOV* (que mueve los archivos y, a continuación, Los elimina del origen).
    >-  No quite las opciones **/e**, **/b**y **/copyall** .

4. Una vez completada la copia, examine el registro en busca de errores o archivos omitidos. Use Robocopy para copiar los archivos omitidos individualmente en lugar de recopiar todo el conjunto de archivos. Si los archivos se omitieron debido a bloqueos exclusivos, intente copiar archivos individuales con Robocopy posteriormente, o bien acepte que esos archivos requieran la replicación por cable mediante la Replicación DFS durante la sincronización inicial.

## <a name="next-step"></a>Paso siguiente

Después de completar la copia inicial y usar Robocopy para resolver problemas con tantos archivos omitidos como sea posible, debe usar el cmdlet **Get-DfsrFileHash** en Windows PowerShell o el comando **Dfsrdiag** para validar los archivos preinicializados comparando hash de archivo en los servidores de origen y de destino. Para obtener instrucciones detalladas [, consulte el paso 2: Valide los archivos preinicializados para](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)replicación DFS.
