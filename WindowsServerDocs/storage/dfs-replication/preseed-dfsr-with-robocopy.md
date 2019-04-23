---
title: Use Robocopy para inicializar previamente los archivos para la replicación DFS
description: Cómo usar Robocopy.exe para inicializar previamente los archivos para la replicación DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865086"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Use Robocopy para inicializar previamente los archivos para la replicación DFS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

En este tema se explica cómo usar la herramienta de línea de comandos, **Robocopy.exe**, para inicializar previamente los archivos al configurar la replicación para la replicación del sistema de archivos distribuido (DFS) (también conocida como DFSR o DFS-R) en Windows Server. Por preinicializar los archivos antes de configurar la replicación DFS, agregue a un nuevo asociado de replicación, o reemplazar un servidor, puede acelerar la sincronización inicial y habilitar la clonación de la base de datos de replicación DFS en Windows Server 2012 R2. El método de Robocopy es uno de los diversos métodos preseeding; Para obtener información general, consulte [paso 1: inicializar previamente los archivos para la replicación DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

La utilidad de línea de comandos de Robocopy (copia de archivos sólido) se incluye con Windows Server. La utilidad proporciona muchas opciones que incluyen la copia seguridad, copia de seguridad API soporte técnico, capacidades de reintento y el registro. Las versiones posteriores incluyen compatibilidad de subprocesamiento múltiple y sin almacenamiento en búfer de E/S.

>[!IMPORTANT]
>Robocopy no copia los archivos bloqueados exclusivamente. Si los usuarios tienden a muchos archivos de bloqueo durante largos períodos en los servidores de archivos, considere el uso de un método preseeding diferente. Preinicializar no requiere a una coincidencia perfecta entre las listas de archivos en los servidores de origen y destino, pero es el número de archivos que no existe cuando se realiza la sincronización inicial para la replicación DFS, el preinicializar menos efectivas. Para minimizar los conflictos de bloqueo, use Robocopy durante horas punta para su organización. Siempre puede examinar los registros de Robocopy después preinicializar para asegurarse de que comprende qué archivos se han omitido debido a bloqueos exclusivos.

Para usar Robocopy para inicializar previamente los archivos para la replicación DFS, siga estos pasos:

1. [Descargue e instale la versión más reciente de Robocopy.](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [Estabilizar los archivos que se van a replicar.](#step-2:-stabilize-files-that-will-be-replicated)
3. [Copie los archivos replicados en el servidor de destino.](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Requisitos previos

Porque preinicializar no implicadas directamente en la replicación DFS, solo deberá cumplir los requisitos para realizar una copia de archivo con Robocopy.

- Se necesita una cuenta que sea miembro del grupo Administradores local en los servidores de origen y destino.

- Instalar la versión más reciente de Robocopy en el servidor que va a utilizar para copiar los archivos, el servidor de origen o el servidor de destino; deberá instalar la versión más reciente para la versión del sistema operativo. Para obtener instrucciones, consulte [paso 2: Los archivos que se van a replicar se estabiliza](#step-2:-stabilize-files-that-will-be-replicated). A menos que se preinicializar los archivos desde un servidor que ejecuta Windows Server 2003 R2, puede ejecutar Robocopy en el servidor de origen o destino. El servidor de destino, que normalmente tiene la versión más reciente del sistema operativo, proporciona acceso a la versión más reciente de Robocopy.

- Asegúrese de que hay suficiente espacio de almacenamiento en la unidad de destino. No cree una carpeta en la ruta de acceso que va a copiar en: Robocopy debe crear la carpeta raíz.
    
    >[!NOTE]
    >Al decidir cuánto espacio asignar para los archivos preseeded, considere la posibilidad de crecimiento esperado de los datos a través de los requisitos de almacenamiento y tiempo para la replicación DFS. Para obtener ayuda, consulte [editar el tamaño de cuota de la carpeta de almacenamiento provisional y conflicto y eliminar carpeta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) en [administrar la replicación DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- En el servidor de origen, opcionalmente, instale Monitor de procesos o Process Explorer, que puede usar para comprobar las aplicaciones que están bloqueando los archivos. Para información sobre la descarga, vea [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) y [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Paso 1: Descargue e instale la versión más reciente de Robocopy

Antes de usar Robocopy para inicializar previamente los archivos, debe descargar e instalar la versión más reciente de **Robocopy.exe**. Esto garantiza que la replicación DFS no omitir archivos debido a problemas de las versiones de Robocopy trasvase de registros.

El origen de la última versión compatible de Robocopy depende de la versión de Windows Server que se está ejecutando en el servidor. Para obtener información acerca de cómo descargar la revisión con la versión más reciente de Robocopy para Windows Server 2008 R2 o Windows Server 2008, consulte [lista de las revisiones disponibles actualmente para las tecnologías de sistema de archivos distribuido (DFS) en Windows Server 2008 y Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Como alternativa, puede buscar e instalar la revisión más reciente para un sistema operativo mediante los pasos siguientes.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Busque e instale la versión más reciente de Robocopy para una versión específica de Windows Server

1. En un explorador web, abra [ https://support.microsoft.com ](https://support.microsoft.com/).

2. En **buscar soporte técnico**, escriba la siguiente cadena, reemplazando `<operating system version>` con el sistema operativo adecuado, a continuación, presione la tecla ENTRAR:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Por ejemplo, escriba **robocopy.exe kbqfe "Windows Server 2008 R2"**.

3. Buscar y descargar la revisión con el mayor número de identificación (es decir, la versión más reciente).

4. Instale la revisión en el servidor.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Paso 2: Estabilizar los archivos que se van a replicar

Después de instalar la versión más reciente de Robocopy en el servidor, debe evitar que los archivos bloqueados copien bloqueo mediante el uso de los métodos descritos en la tabla siguiente. La mayoría de las aplicaciones no bloquean exclusivamente los archivos. Sin embargo, durante las operaciones normales, puede estar bloqueado un pequeño porcentaje de los archivos en servidores de archivos.

|Origen del bloqueo|Explicación|Solución|
|---|---|---|
|Los usuarios abrir remotamente archivos en recursos compartidos.|Los empleados conectan a un servidor de archivos estándar y edición documentos, contenido multimedia u otros archivos. Conoce a veces como la tradicional carpeta particular o cargas de trabajo de datos compartido.|Solo las operaciones de Robocopy durante el pico, fuera del horario laboral. Esto minimiza el número de archivos que se debe omitir Robocopy durante preinicializar.<br><br>Considere la posibilidad de establecer temporalmente el acceso de solo lectura en los recursos compartidos de archivo que se van a replicar mediante el uso de Windows PowerShell **Grant SmbShareAccess** y **cerrar SmbSession** cmdlets. Si establece permisos para un grupo comunes, como todos los usuarios o a usuarios autenticados para lectura, los usuarios estándar podrían ser menos probables abrir archivos con bloqueos exclusivos (si sus aplicaciones detectan el acceso de solo lectura cuando se abren archivos).<br><br>También se puede establecer una regla de firewall temporal para el puerto SMB 445 entrante a ese servidor para bloquear el acceso a archivos o utilizar el **bloque SmbShareAccess** cmdlet. Sin embargo, ambos de estos métodos son muy problemática para operaciones de usuario.|
|Aplicaciones abren archivos locales.|Las cargas de trabajo de aplicación que se ejecuta en un servidor de archivos a veces bloquean los archivos.|Deshabilitar temporalmente o desinstalar las aplicaciones que están bloqueando los archivos. Puede usar el Monitor de procesos o el Explorador de procesos para determinar qué aplicaciones están bloqueando los archivos. Para descargar el Monitor de procesos o Process Explorer, visite la [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) y [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) páginas.|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Paso 3: Copie los archivos replicados en el servidor de destino

Después de reducir los bloqueos de los archivos que se van a replicar, puede inicializar previamente los archivos del servidor de origen al servidor de destino.

>[!NOTE]
>Puede ejecutar Robocopy en el equipo de origen o el equipo de destino. El siguiente procedimiento describe cómo ejecutar Robocopy en el servidor de destino, que normalmente se está ejecutando un sistema operativo más reciente, para aprovechar las capacidades adicionales de Robocopy que puede proporcionar el sistema operativo más reciente.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Inicializar previamente los archivos replicados en el servidor de destino con Robocopy

1. Inicie sesión en el servidor de destino con una cuenta que sea miembro del grupo Administradores local en los servidores de origen y destino.

2. Abre un símbolo del sistema con privilegios elevados.

3. Para inicializar previamente los archivos desde el origen al servidor de destino, ejecute el siguiente comando, sustituyendo su propio origen, destino y las rutas de acceso del archivo de registro para los valores entre corchetes:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Este comando copia todo el contenido de la carpeta de origen a la carpeta de destino, con los siguientes parámetros:
    
    |Parámetro|Descripción|
    |---|---|
    |"\<origen replica la ruta de acceso de carpeta\>"|Especifica la carpeta de origen para inicializar previamente en el servidor de destino.|
    |"\<destino replica la ruta de acceso de carpeta\>"|Especifica la ruta de acceso a la carpeta donde se almacenará los archivos preseeded.<br><br>La carpeta de destino no debe existir en el servidor de destino. Para obtener el hash de archivo coincidente, Robocopy debe crear la carpeta raíz cuando lo preseeds los archivos.|
    |/e|Copia los subdirectorios y sus archivos, así como los subdirectorios vacíos.|
    |/b|Copia los archivos en modo de copia de seguridad.|
    |/ copyal|Copia toda información de archivo, incluidos los datos, atributos, marcas de tiempo, la lista de control de acceso (ACL) de NTFS, información de propietario e información de auditoría.|
    |/r:6|Vuelve a intentar la operación seis veces cuando se produce un error.|
    |/w:5|Espera 5 segundos entre reintentos.|
    |MT:64|Copia 64 archivos simultáneamente.|
    |/xd DfsrPrivate|Excluye la carpeta DfsrPrivate.|
    |/tee|Escribe la salida del estado de la ventana de consola, así como el archivo de registro.|
    |o en el registro \<ruta de acceso de archivo de registro >|Especifica el archivo de registro para escribir. Sobrescribe el contenido del archivo existente. (Para anexar las entradas al archivo de registro existente, utilice `/log+ <log file path>`.)|
    |/v|Genera un resultado detallado que incluye archivos omitidos.|
    
    Por ejemplo, el siguiente comando replica los archivos de la carpeta de origen se replican, E:\\RF01, en la unidad de datos D en el servidor de destino:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Se recomienda que utilice los parámetros que se ha descrito anteriormente, al usar Robocopy para inicializar previamente los archivos para la replicación DFS. Sin embargo, puede cambiar algunos de sus valores o agregar parámetros adicionales. Por ejemplo, es posible que descubra a través de pruebas que tienen la capacidad para establecer un valor mayor (número de subprocesos) para el */MT* parámetro. Además, si se van a replicar principalmente archivos de mayor tamaño, es posible que pueda aumentar el rendimiento de la copia agregando el **/j** opción de E/S no almacenada en búfer. Para obtener más información acerca de los parámetros de Robocopy, consulte el [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) referencia de línea de comandos.

    >[!WARNING]
    >Para evitar la posible pérdida de datos al usar Robocopy para inicializar previamente los archivos para la replicación DFS, no hace los siguientes cambios a los parámetros recomendados:
    >- No utilice el */mir* parámetro (que refleja un árbol de directorios) o el */mov* parámetro (que mueve los archivos y luego los elimina del origen).
    >-  No quite el **/e**, **/b**, y **/copyall** opciones.

4. Una vez finalizada la copia, examine el registro para los errores o archivos omitidos. Use Robocopy para copiar los archivos omitidos individualmente en lugar de volver a copiar todo el conjunto de archivos. Si los archivos se han omitido debido a bloqueos exclusivos, intente copiar los archivos individuales con Robocopy más tarde o acepte el que esos archivos requerirá over el cable replicación para la replicación DFS durante la sincronización inicial.

## <a name="next-step"></a>Paso siguiente

Después de completar la copia inicial y use Robocopy para resolver problemas relacionados con archivos omitidos tantos como sea posible, usará el **Get DfsrFileHash** cmdlet de Windows PowerShell o la **Dfsrdiag** comando Valide los archivos preseeded mediante la comparación de hash de archivo en los servidores de origen y destino. Para obtener instrucciones detalladas, consulte [paso 2: Validar archivos Preseeded para la replicación DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).