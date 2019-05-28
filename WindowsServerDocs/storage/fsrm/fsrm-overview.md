---
title: Introducción al Administrador de recursos del servidor de archivos (FSRM)
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: Administrador de recursos de servidor de archivos (FSRM) es una herramienta que le permite administrar y clasificar los datos en un servidor de archivos de Windows Server.
ms.openlocfilehash: 8488c7418ac03be53db7164678fad353bc7c637d
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476126"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Introducción al Administrador de recursos del servidor de archivos (FSRM)

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semianual), 

El Administrador de recursos del servidor (FSRM) es un servicio de rol de Windows Server que te permite administrar y clasificar los datos almacenados en los servidores de archivos. Puede usar el Administrador de recursos del servidor de archivos para clasificar archivos, realizar tareas basadas en estas clasificaciones, establecer cuotas en carpetas y crear informes de supervisión del uso de almacenamiento automáticamente.

También es un punto de pequeño, pero debemos [agregó la capacidad de deshabilitar cambios diarios](#whats-new) en Windows Server, versión 1803.

## <a name="features"></a>Características

El Administrador de recursos del servidor de archivos incluye las siguientes características:

-   [Administración de cuotas](quota-management.md) le permite limitar el espacio permitido para un volumen o carpeta, y se pueden aplicar automáticamente a las nuevas carpetas que se crean en un volumen. También puede definir plantillas de cuota que se apliquen a los nuevos volúmenes o carpetas.  
-   [La infraestructura de clasificación de archivos](classification-management.md) proporciona información sobre los datos automatizando los procesos de clasificación para que pueda administrar los datos de forma más eficaz. Puede clasificar archivos y aplicar directivas según esta clasificación. Entre las directivas de ejemplo está el control de acceso dinámico para restringir el acceso a archivos, el cifrado de archivos y la expiración de archivos. Los archivos pueden clasificarse de manera automática mediante reglas de clasificación de archivos o de forma manual, modificando las propiedades de un archivo o carpeta que se haya seleccionado.
-   [Tareas de administración de archivos](file-management-tasks.md) le permite aplicar una acción o directiva condicional en los archivos según su clasificación. Entre las condiciones de una tarea de administración de archivos están la ubicación del archivo, las propiedades de clasificación, la fecha en que se creó el archivo, la fecha de la última modificación del archivo o la última vez que se accedió al archivo. Las acciones que una tarea de administración de archivos puede llevar a cabo incluyen la capacidad de expirar archivos, cifrar archivos o ejecutar un comando personalizado.
-   [Administración del filtrado de archivo](file-screening-management.md) le permite controlar los tipos de archivos que el usuario puede almacenar en un servidor de archivos. Puede limitar la extensión que se puede almacenar en los archivos compartidos. Por ejemplo, puede crear un filtrado de archivos que no permita que se almacenen archivos con extensión MP3 en las carpetas compartidas personales en un servidor de archivos.
-   [Informes de almacenamiento](storage-reports-management.md) le ayudarán a identificar las tendencias de uso de disco y cómo se clasifican los datos. También puede supervisar un grupo de usuarios en concreto para detectar los intentos de guardar archivos no autorizados.  
  
Las características incluidas con el Administrador de recursos del servidor de archivos se pueden configurar y administrar mediante la aplicación Administrador de recursos del servidor de archivos o usando Windows PowerShell.
  
> [!IMPORTANT]
>  El Administrador de recursos del servidor de archivos admite únicamente volúmenes que estén formateados con el sistema de archivos NTFS. El sistema de archivos resistente no se admite.  
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
 Entre las aplicaciones prácticas del Administrador de recursos del servidor de archivos se incluyen las siguientes:  
  
-   Use la infraestructura de clasificación de archivos con el escenario de control de acceso dinámico para crear una directiva que conceda acceso a los archivos y carpetas en función del modo en que se clasifican los archivos en el servidor de archivos.  
  
-   Cree una regla de clasificación de archivos que etiquete cualquier archivo que contenga al menos 10 números de la seguridad social como archivo que contiene información de identificación personal.  
  
-   Establezca la expiración de cualquier archivo que no se haya modificado en los últimos 10 años.  
  
-   Establezca una cuota de 200 megabytes para el directorio principal de cada usuario y envíeles una notificación cuando lleguen a los 180 megabytes de uso.  
  
-   No deje almacenar archivos de música en las carpetas personales compartidas.  
  
-   Programe que todos los domingos a medianoche se ejecute un informe que genere una lista de los últimos archivos a los que se ha accedido en los dos últimos días. Este informe puede servir de ayuda para conocer la actividad de almacenamiento durante el fin de semana y planear el tiempo de inactividad del servidor según esa información.  

## <a name="whats-new"></a>Novedades: evitar que FSRM crear diarios de cambios

A partir de Windows Server, versión 1803, puede evitar ahora el servicio Administrador de recursos del servidor de archivos desde la creación de un diario de cambios (también conocido como un diario USN) en volúmenes cuando se inicia el servicio. Esto puede ahorrar un poco de espacio en cada volumen, pero deshabilitará la clasificación de archivos en tiempo real.

Para las características nuevas más antiguas, consulte [Novedades en el Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/dn383587.aspx).

Para evitar que crear un diario de cambios en algunos o todos los volúmenes cuando se inicia el servicio Administrador de recursos del servidor de archivos, siga estos pasos: 

1. Detenga el servicio SRMSVC. Por ejemplo, abra una sesión de PowerShell como administrador y escriba `Stop-Service SrmSvc`.
2. Eliminar el diario USN para los volúmenes que desea conservar el espacio en mediante el comando fsutil: 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    Por ejemplo: `fsutil usn deletejournal /d c:`

3. Abra el Editor del registro, por ejemplo, escribiendo `regedit` en la misma sesión de PowerShell.
4. Vaya a la siguiente clave: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. Si lo desea para omitir cambie la creación de diario para todo el servidor (omitir este paso si desea deshabilitarlo sólo en volúmenes específicos):
    1. Haga clic en el **configuración** clave y, a continuación, seleccione **New** > **valor DWORD (32 bits)** . 
    1. Asigne el valor `SkipUSNCreationForSystem`.
    1. Establezca el valor en **1** (en hexadecimal).
6. Para omitir la creación del diario de cambios para volúmenes específicos:
    1. Obtener las rutas de acceso del volumen que desea omitir mediante el `fsutil volume list` comando o el siguiente comando de PowerShell:
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       Este es un ejemplo de salida:

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. Nuevo en el Editor del registro, haga clic en el **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings** clave y, a continuación, seleccione **New** > **cadena múltiple Valor**.
    3. Asigne el valor `SkipUSNCreationForVolumes`.
    4. Escriba la ruta de acceso de cada volumen en el que se omiten la creación de un diario de cambios, coloque cada ruta de acceso en una línea independiente. Por ejemplo:

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > Editor del registro podría indicarle que quite las cadenas vacías, mostrar esta advertencia que puede omitir de forma segura: *Datos de tipo REG_MULTI_SZ no pueden contener cadenas vacías. Editor del registro se quitarán todas las cadenas vacías que se encuentra.*

7. Inicie el servicio SRMSVC. Por ejemplo, en una sesión de PowerShell, escriba `Start-Service SrmSvc`.



## <a name="see-also"></a>Vea también

- [Control de acceso dinámico](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 