---
title: Uso de Robocopy para preinicializar archivos para Replicación DFS
description: Cómo usar Robocopy.exe para preinicializar archivos para Replicación DFS.
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 005df7b7cdf14c730d556585141891aa78ba714d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971202"
---
# <a name="use-robocopy-to-pre-seed-files-for-dfs-replication"></a>Uso de Robocopy para preinicializar archivos para Replicación DFS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

En este tema se explica cómo usar la herramienta de línea de comandos **Robocopy.exe** para preinicializar los archivos al configurar la replicación del Sistema de archivos distribuido (DFS) (también conocida como DFSR o DFS-R) en Windows Server. Al preinicializar los archivos antes de configurar la Replicación DFS, agregar un nuevo partner de replicación o reemplazar un servidor, puedes acelerar la sincronización inicial y habilitar la clonación de la base de datos de Replicación DFS en Windows Server 2012 R2. El método Robocopy es uno de varios métodos de preinicialización; para obtener información general, consulta [Paso 1: Preinicializar archivos para Replicación DFS](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

La utilidad de línea de comandos Robocopy (copia eficaz de archivos) se incluye con Windows Server. La utilidad proporciona amplias opciones que incluyen la seguridad las copias, la compatibilidad con la API de copia de seguridad, funcionalidades de reintento y registro. Las versiones posteriores incluyen compatibilidad con subprocesamiento múltiple y E/S sin almacenamiento en búfer.

>[!IMPORTANT]
>Robocopy no copia archivos bloqueados exclusivamente. Si los usuarios suelen bloquear muchos archivos durante largos períodos en los servidores de archivos, considera la posibilidad de usar otro método de preinicialización. La preinicialización no requiere una coincidencia perfecta entre las listas de archivos en los servidores de origen y de destino, pero cuantos más archivos no existan cuando se hace la sincronización inicial para Replicación DFS, menos efectiva será la preinicialización. Para minimizar los conflictos de bloqueo, usa Robocopy durante las horas de poca actividad de tu organización. Examina siempre los registros de Robocopy después de la preinicialización para asegurarte de comprender qué archivos se omitieron debido a bloqueos exclusivos.

Para usar Robocopy para preinicializar archivos para Replicación DFS, sigue estos pasos:

1. [Descargar e instalar la versión más reciente de Robocopy.](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [Estabilizar los archivos que se van a replicar.](#step-2-stabilize-files-that-will-be-replicated)
3. [Copiar los archivos replicados en el servidor de destino.](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Requisitos previos

Dado que la preinicialización no implica directamente la Replicación DFS, solo tienes que cumplir los requisitos para hacer una copia de archivos con Robocopy.

- Necesitas una cuenta que sea miembro del grupo de administradores locales tanto en el servidor de origen como en el de destino.

- Instala la versión más reciente de Robocopy en el servidor que vas a usar para copiar los archivos, ya sea el servidor de origen o el servidor de destino. Tendrás que instalar la versión más reciente del sistema operativo. Para obtener instrucciones, consulta el [Paso 2: Estabilizar los archivos que se van a replicar](#step-2-stabilize-files-that-will-be-replicated). A menos que estés preinicializando archivos de un servidor que ejecute Windows Server 2003 R2, puedes ejecutar Robocopy en el servidor de origen o de destino. El servidor de destino, que normalmente tiene la versión más reciente del sistema operativo, te da acceso a la versión más reciente de Robocopy.

- Asegúrate de que haya suficiente espacio de almacenamiento disponible en la unidad de destino. No crees una carpeta en la ruta de acceso en la que vas a hacer la copia: Robocopy tiene que crear la carpeta raíz.

    >[!NOTE]
    >Cuando decidas cuánto espacio quieres asignar para los archivos preinicializados, ten en cuenta el crecimiento esperado de los datos a lo largo del tiempo y los requisitos de almacenamiento para Replicación DFS. Como ayuda para la planificación, consulta [Editar el tamaño de cuota de la carpeta de almacenamiento provisional y la carpeta Conflictos y eliminaciones](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc754229(v=ws.11)) en [Administración de Replicación DFS](</previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Como opción, en el servidor de origen instala Process Monitor o Process Explorer, que puedes usar para comprobar si hay aplicaciones que están bloqueando archivos. Para obtener información sobre la descarga, consulta [Process Monitos](/sysinternals/downloads/procmon) y [Process Explorer](/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Paso 1: Descargar e instalar la versión más reciente de Robocopy

Antes de usar Robocopy para preinicializar archivos, tienes que descargar e instalar la versión más reciente de **Robocopy.exe**. Esto garantiza que Replicación DFS no omita archivos debido a problemas en las versiones de envío de Robocopy.

El origen de la versión más reciente de Robocopy compatible depende de la versión de Windows Server que se ejecute en el servidor. Para obtener información sobre cómo descargar la revisión con la versión más reciente de Robocopy para Windows Server 2008 R2 o Windows Server 2008, consulta [Lista de revisiones disponibles actualmente para las tecnologías de sistema de archivos distribuido (DFS) en Windows Server 2008 y en Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Como alternativa, puedes buscar e instalar la revisión más reciente para un sistema operativo siguiendo los pasos a continuación.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Búsqueda e instalación de la versión más reciente de Robocopy para una versión específica de Windows Server

1. En un explorador web, abre [https://support.microsoft.com](https://support.microsoft.com/).

2. En **Buscar ayuda**, escribe la cadena siguiente, reemplazando `<operating system version>` por el sistema operativo adecuado y, a continuación, presiona la tecla Entrar:

    ```robocopy.exe kbqfe "<operating system version>"```

    Por ejemplo, escribe **Robocopy.exe kbqfe "Windows Server 2008 R2"** .

3. Busca y descarga la revisión con el número de identificación más alto (es decir, la versión más reciente).

4. Instala la revisión en el servidor.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Paso 2: Estabilizar los archivos que se van a replicar

Después de instalar la versión más reciente de Robocopy en el servidor, tienes que evitar que los archivos bloqueados impidan la copia; para ello, usa los métodos descritos en la tabla siguiente. La mayoría de las aplicaciones no bloquea exclusivamente los archivos. Sin embargo, durante las operaciones normales, es posible que un pequeño porcentaje de archivos quede bloqueado en los servidores de archivos.

|Origen del bloqueo|Explicación|Solución|
|---|---|---|
|Los usuarios abren archivos en recursos compartidos de forma remota.|Los empleados se conectan a un servidor de archivos estándar y editan documentos, contenido multimedia u otros archivos. A veces se conoce como la carpeta de inicio tradicional o cargas de trabajo de datos compartidos.|Completa las operaciones de Robocopy únicamente fuera de las horas pico y fuera del horario comercial. Esto minimiza el número de archivos que Robocopy debe omitir durante la preinicialización.<br><br>Piensa en la posibilidad de configurar temporalmente el acceso de solo lectura en los recursos compartidos de archivos que se van a replicar con los cmdlets de Windows PowerShell **Grant-SmbShareAccess** y **Close-SmbSession**. Si estableces permisos de Lectura para un grupo común, como Todos o Usuarios autenticados, es menos probable que los usuarios estándar abran archivos con bloqueos exclusivos (si sus aplicaciones detectan el acceso de solo lectura cuando se abren archivos).<br><br>También puedes considerar la posibilidad de establecer una regla de firewall temporal para el puerto 445 de entrada de SMB a ese servidor para bloquear el acceso a los archivos o usar el cmdlet **Block-SmbShareAccess**. Sin embargo, ambos métodos son muy molestos para las operaciones de los usuarios.|
|Las aplicaciones abren archivos localmente.|Las cargas de trabajo de aplicaciones que se ejecutan en un servidor de archivos a veces bloquean los archivos.|Deshabilita o desinstala temporalmente las aplicaciones que bloquean los archivos. Puedes usar Process Monitor o Process Explorer para determinar qué aplicaciones están bloqueando archivos. Para descargar Process Monitor o Process Explorer, visita las páginas de [Process Explorer](/sysinternals/downloads/procmon) y [Process Monitor](/sysinternals/downloads/process-explorer).|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Paso 3: Copiar los archivos replicados en el servidor de destino

Después de minimizar los bloqueos en los archivos que se van a replicar, puedes preinicializar los archivos del servidor de origen en el servidor de destino.

>[!NOTE]
>Puedes ejecutar Robocopy en el equipo de origen o en el equipo de destino. En el procedimiento siguiente se describe la ejecución de Robocopy en el servidor de destino, que normalmente ejecuta un sistema operativo más reciente, para aprovechar las funcionalidades adicionales de Robocopy que pueda proporcionar el sistema operativo más reciente.

### <a name="pre-seed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Preinicialización de los archivos replicados en el servidor de destino con Robocopy

1. Inicia sesión en el servidor de destino con una cuenta que sea miembro del grupo de administradores locales tanto en el servidor de origen como en el de destino.

2. Abra un símbolo del sistema con privilegios elevados.

3. Para preinicializar los archivos del servidor de origen al de destino, ejecuta el siguiente comando, sustituyendo los valores entre llaves por tus propias rutas de acceso de los archivos de origen, destino y registro:

    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```

    Este comando copia todo el contenido de la carpeta de origen en la carpeta de destino, con los parámetros siguientes:

    |Parámetro|Descripción|
    |---|---|
    |"\<source replicated folder path\>"|Especifica la carpeta de origen que se va a preinicializar en el servidor de destino.|
    |"\<destination replicated folder path\>"|Especifica la ruta de acceso a la carpeta que almacenará los archivos preinicializados.<br><br>La carpeta de destino no debe existir ya en el servidor de destino. Para obtener los hash de archivo coincidentes, Robocopy tiene que crear la carpeta raíz cuando se preinicializan los archivos.|
    |/e|Copia los subdirectorios y sus archivos, así como los subdirectorios vacíos.|
    |/b|Copia los archivos en modo de copia de seguridad.|
    |/copyall|Copia la información de todos los archivos, incluidos los datos, los atributos, las marcas de tiempo, la lista de control de acceso (ACL) de NTFS, la información de propietarios y la información de auditoría.|
    |/r:6|Vuelve a intentar la operación seis veces cuando se produce un error.|
    |/w:5|Espera 5 segundos entre los reintentos.|
    |MT:64|Copia 64 archivos simultáneamente.|
    |/xd DfsrPrivate|Excluye la carpeta DfsrPrivate.|
    |/tee|Escribe la salida de estado en la ventana de la consola, así como en el archivo de registro.|
    |/log \<log file path>|Especifica el archivo de registro que se va a escribir. Sobrescribe el contenido existente del archivo. (Para anexar las entradas al archivo de registro existente, usa `/log+ <log file path>`).|
    |/v|Genera una salida detallada que incluye los archivos omitidos.|

    Por ejemplo, el siguiente comando replica los archivos de la carpeta replicada de origen, E:\\RF01, en la unidad de datos D del servidor de destino:

    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\pre-seedsrv02.log
    ```

    >[!NOTE]
    >Se recomienda usar los parámetros descritos anteriormente al usar Robocopy para preinicializar archivos para Replicación DFS. Sin embargo, puedes cambiar algunos de sus valores o agregar parámetros adicionales. Por ejemplo, puede que mediante pruebas descubras que tienes la capacidad de establecer un valor mayor (recuento de subprocesos) para el parámetro */MT*. Además, si principalmente vas a replicar archivos de mayor tamaño, es posible que puedas aumentar el rendimiento de las copias si agregas la opción **/j** para la E/S no almacenada en búfer. Para obtener más información acerca de los parámetros de Robocopy, consulta la referencia de la línea de comandos de [Robocopy](../../administration/windows-commands/robocopy.md).

    >[!WARNING]
    >Para evitar la posible pérdida de datos cuando uses Robocopy para preinicializar archivos para Replicación DFS, no hagas los cambios siguientes en los parámetros recomendados:
    >- No uses el parámetro */mir* (que refleje un árbol de directorios) ni el parámetro */mov* (que mueve los archivos y, luego, los elimina del origen).
    >-  No quites las opciones **/e**, **/b** y **/copyall**.

4. Una vez completada la copia, examina el registro en busca de errores o archivos omitidos. Usa Robocopy para copiar individualmente los archivos omitidos, en lugar de volver a copiar todo el conjunto de archivos. Si los archivos se omitieron debido a bloqueos exclusivos, intenta copiar los archivos individuales con Robocopy más adelante, o bien acepta que esos archivos requerirán la replicación por cable mediante la Replicación DFS durante la sincronización inicial.

## <a name="next-step"></a>Paso siguiente

Después de completar la copia inicial y usar Robocopy para resolver los problemas con la mayor cantidad de archivos omitidos como sea posible, usarás el cmdlet **Get-DfsrFileHash** de Windows PowerShell o el comando **Dfsrdiag** para validar los archivos preinicializados al comparar los hash de archivo en los servidores de origen y de destino. Para obtener instrucciones detalladas, consulta [Paso 2: Validar archivos preinicializados para Replicación DFS](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).
