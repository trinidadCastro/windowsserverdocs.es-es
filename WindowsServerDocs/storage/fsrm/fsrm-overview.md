---
title: Información general de Administrador de recursos de servidor de archivos (FSRM)
ms.prod: windows-server
ms.author: jgerend
manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: Administrador de recursos de servidor de archivos (FSRM) es una herramienta que permite administrar y clasificar los datos en un servidor de archivos de Windows Server.
ms.openlocfilehash: af54f08f8acc491553a4d42c1aabe8ea7e26fadd
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473942"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Información general de Administrador de recursos de servidor de archivos (FSRM)

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semianual),

Administrador de recursos de servidor de archivos (FSRM) es un servicio de rol de Windows Server que permite administrar y clasificar los datos almacenados en los servidores de archivos. Puede usar Administrador de recursos de servidor de archivos para clasificar archivos automáticamente, realizar tareas basadas en estas clasificaciones, establecer cuotas en carpetas y crear informes que supervisan el uso del almacenamiento.

Es un punto pequeño, pero también hemos [agregado la posibilidad de deshabilitar los diarios de cambios](#whats-new) en Windows Server, versión 1803.

## <a name="features"></a>Características

El Administrador de recursos del servidor de archivos incluye las siguientes características:

-   La [Administración de cuotas](quota-management.md) permite limitar el espacio permitido para un volumen o una carpeta, y se pueden aplicar automáticamente a las nuevas carpetas que se creen en un volumen. También puede definir plantillas de cuota que se apliquen a los nuevos volúmenes o carpetas.
-   La [infraestructura de clasificación de archivos](classification-management.md) proporciona una visión general de los datos mediante la automatización de los procesos de clasificación para que pueda administrar los datos de forma más eficaz. Puede clasificar archivos y aplicar directivas según esta clasificación. Entre las directivas de ejemplo está el control de acceso dinámico para restringir el acceso a archivos, el cifrado de archivos y la expiración de archivos. Los archivos pueden clasificarse de manera automática mediante reglas de clasificación de archivos o de forma manual, modificando las propiedades de un archivo o carpeta que se haya seleccionado.
-   [Las tareas de administración de archivos](file-management-tasks.md) permiten aplicar una acción o directiva condicional a los archivos en función de su clasificación. Entre las condiciones de una tarea de administración de archivos están la ubicación del archivo, las propiedades de clasificación, la fecha en que se creó el archivo, la fecha de la última modificación del archivo o la última vez que se accedió al archivo. Las acciones que una tarea de administración de archivos puede llevar a cabo incluyen la capacidad de expirar archivos, cifrar archivos o ejecutar un comando personalizado.
-   La [Administración del filtrado de archivos](file-screening-management.md) le ayuda a controlar los tipos de archivos que el usuario puede almacenar en un servidor de archivos. Puede limitar la extensión que se puede almacenar en los archivos compartidos. Por ejemplo, puede crear un filtrado de archivos que no permita que se almacenen archivos con extensión MP3 en las carpetas compartidas personales en un servidor de archivos.
-   Los [informes de almacenamiento](storage-reports-management.md) le ayudan a identificar las tendencias en el uso de disco y el modo en que se clasifican los datos. También puede supervisar un grupo de usuarios en concreto para detectar los intentos de guardar archivos no autorizados.

Las características incluidas en el servidor de archivos Administrador de recursos se pueden configurar y administrar mediante la aplicación de Administrador de recursos de servidor de archivos o mediante Windows PowerShell.

> [!IMPORTANT]
>  El Administrador de recursos del servidor de archivos admite únicamente volúmenes que estén formateados con el sistema de archivos NTFS. No se admite el sistema de archivos resistente.

## <a name="practical-applications"></a>Aplicaciones prácticas
 Entre las aplicaciones prácticas del Administrador de recursos del servidor de archivos se incluyen las siguientes:

-   Use la infraestructura de clasificación de archivos con el escenario de control de acceso dinámico para crear una directiva que conceda acceso a los archivos y carpetas en función del modo en que se clasifican los archivos en el servidor de archivos.

-   Cree una regla de clasificación de archivos que etiquete cualquier archivo que contenga al menos 10 números de la seguridad social como archivo que contiene información de identificación personal.

-   Establezca la expiración de cualquier archivo que no se haya modificado en los últimos 10 años.

-   Cree una cuota de 200 megabytes para el directorio principal de cada usuario y notifíquelo cuando utilicen 180 megabytes.

-   No deje almacenar archivos de música en las carpetas personales compartidas.

-   Programe que todos los domingos a medianoche se ejecute un informe que genere una lista de los últimos archivos a los que se ha accedido en los dos últimos días. Este informe puede servir de ayuda para conocer la actividad de almacenamiento durante el fin de semana y planear el tiempo de inactividad del servidor según esa información.

## <a name="whats-new---prevent-fsrm-from-creating-change-journals"></a><a name="whats-new"></a>Novedades: impedir que FSRM cree diarios de cambios

A partir de Windows Server, versión 1803, ahora puede impedir que el servidor de archivos Administrador de recursos servicio cree un diario de cambios (también conocido como un diario USN) en los volúmenes cuando se inicia el servicio. Esto puede ahorrar un poco de espacio en cada volumen, pero deshabilitará la clasificación de archivos en tiempo real.

Para conocer las nuevas características anteriores, consulte [novedades en el administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/dn383587.aspx).

Para evitar que el servidor de archivos Administrador de recursos cree un diario de cambios en algunos o en todos los volúmenes cuando se inicie el servicio, siga estos pasos:

1. Detenga el servicio SRMSVC. Por ejemplo, abra una sesión de PowerShell como administrador y escriba `Stop-Service SrmSvc` .
2. Elimine el diario USN para los volúmenes en los que desea ahorrar espacio mediante el comando fsutil:

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    Por ejemplo: `fsutil usn deletejournal /d c:`

3. Abra el editor del registro, por ejemplo, escribiendo `regedit` en la misma sesión de PowerShell.
4. Vaya a la clave siguiente: **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\srmsvc\settings**
5. Para omitir opcionalmente la creación del diario de cambios para todo el servidor (Omita este paso si desea deshabilitarlo solo en volúmenes específicos):
    1. Haga clic con el botón secundario en la clave de **configuración** y seleccione **nuevo**  >  **valor DWORD (32 bits)**.
    1. Asigne un nombre al valor `SkipUSNCreationForSystem` .
    1. Establezca el valor en **1** (en hexadecimal).
6. Para omitir opcionalmente la creación del diario de cambios para volúmenes específicos:
    1. Obtenga las rutas de acceso de volumen que desea omitir mediante el `fsutil volume list` comando o el siguiente comando de PowerShell:
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       A continuación se muestra un ejemplo de salida:

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. De nuevo en el editor del registro, haga clic con el botón derecho en la clave **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\srmsvc\settings** y seleccione **nuevo**  >  **valor de cadena múltiple**.
    3. Asigne un nombre al valor `SkipUSNCreationForVolumes` .
    4. Escriba la ruta de acceso de cada volumen en el que omitir la creación de un diario de cambios y coloque cada trazado en una línea independiente. Por ejemplo:

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE]
        > El editor del registro puede indicarle que quitó cadenas vacías y que muestra esta advertencia sin ningún problema: los *datos de tipo REG_MULTI_SZ no pueden contener cadenas vacías. El editor del registro quitará todas las cadenas vacías encontradas.*

7. Inicie el servicio SRMSVC. Por ejemplo, en una sesión de PowerShell, escriba `Start-Service SrmSvc` .



## <a name="additional-references"></a>Referencias adicionales

- [Access Control dinámico](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx)