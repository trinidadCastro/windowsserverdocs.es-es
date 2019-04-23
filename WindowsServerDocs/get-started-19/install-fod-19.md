---
title: Característica de compatibilidad de aplicación de Server Core a petición (FOD)
description: Cómo instalar características de Windows Server a petición
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: b8211ace56aa6565295a15adce26a8dfbc98e1e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866116"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Característica de compatibilidad de aplicación de Server Core a petición (FOD)

> Se aplica a Windows Server 2019 y Windows Server, versión 1809

El **característica de compatibilidad de Server Core App petición** es un paquete de características opcionales que se puede agregar a las instalaciones Server Core de Windows Server de 2019, o Windows Server, versión 1809, en cualquier momento.

Para obtener más información sobre las características en la demanda (du), consulte [características a petición](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).


## <a name="why-install-the-app-compatibility-fod"></a>¿Por qué instalar los DU de compatibilidad de la aplicación? 

Compatibilidad de aplicaciones, una característica a petición para Server Core, mejora significativamente la compatibilidad de aplicaciones de la opción de instalación Server Core de Windows mediante la inclusión de un subconjunto de los archivos binarios y paquetes de Windows Server con experiencia de escritorio, sin agregar el Entorno gráfico de la experiencia de escritorio de Windows Server. Este paquete opcional está disponible en un archivo ISO independiente, o desde Windows Update, pero solo puede agregarse a las imágenes y las instalaciones Server Core de Windows.

Los dos valores principales que proporciona los DU de compatibilidad de aplicación son:

1.  Aumenta la compatibilidad de Server Core para aplicaciones de servidor que ya están en el mercado o ya se ha desarrollado por organizaciones e implementado.

2.  Ayuda con la prestación de los componentes del sistema operativo y aplicación una mayor compatibilidad de herramientas de software usado aguda para solucionar problemas y escenarios de depuración.

Componentes del sistema operativo que están disponibles como parte de los DU de compatibilidad de Server Core App incluyen:

-   Microsoft Management Console (mmc.exe)

-   Visor de eventos (Eventvwr.msc)

-   Monitor de rendimiento (PerfMon.exe)

-   Monitor de recursos (Resmon.exe)

-   Administrador de dispositivos (Devmgmt.msc)

-   Explorador de archivos (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Administración de discos (Diskmgmt.msc)

-   Administrador de clústeres de conmutación por error (CluAdmin.msc)

    -   Requiere la adición de la característica de Windows Server de agrupación en clústeres de conmutación por error en primer lugar.

        -   Use el Cmdlet de Powershell para agregar, `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   Para ejecutar el Administrador de clústeres de conmutación por error, escriba **cluadmin** en el símbolo del sistema.

## <a name="installing-the-app-compatibility-fod"></a>Instalar la compatibilidad de aplicaciones du

 >[!IMPORTANT] 
   >Los DU de compatibilidad de la aplicación solo puede instalarse en Server Core. No intente agregar los DU de compatibilidad de Server Core App a una instalación de Windows Server de Windows Server con experiencia de escritorio.

### <a name="to-add-the-server-core-app-compatibility-feature-on-demand-fod-to-a-running-instance-of-server-core"></a>Para agregar la característica de compatibilidad de aplicaciones de Server Core bajo demanda (du) a una instancia en ejecución de Server Core

 >[!NOTE] 
   > Este procedimiento utiliza el mantenimiento de imágenes de implementación y administración (DISM.exe), una herramienta de línea de comandos. Para obtener más información acerca de los comandos DISM, vea [paquete capacidades de DISM mantenimiento Command-Line Options](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options).

>[!NOTE] 
   > Los mismos paquetes opcionales de du ISO pueden utilizarse para las instalaciones Server Core de Windows Server de 2019, o Windows Server, versión 1809, las instalaciones.

>[!NOTE] 
   > Si el equipo o máquina virtual que ejecuta Server Core es capaz de conectarse a Windows Update, puede omitir los pasos 1-7, a continuación. Pero asegúrese de dejar desactivada/Source y /LimitAccess desde el comando DISM en el paso 8.

1. Descargar los paquetes opcionales del servidor du ISO y copie la imagen ISO a una carpeta compartida en la red local:

 - Si tiene una licencia por volumen, que puede descargar el archivo de imagen ISO de du Server desde el mismo portal donde se obtiene el archivo de imagen ISO del sistema operativo: [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
 - El archivo de imagen ISO de du Server también está disponible en el [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) o en el [portal de Visual Studio](https://visualstudio.microsoft.com) para los suscriptores.


2. Inicie sesión como administrador en el equipo que está conectado a la red local y que desea agregar los DU para Server Core.

3. Usar **net use**, o algún otro método para conectarse a la ubicación de la ISO FOD.

4. Copie FOD ISO en una carpeta local de su elección.

5. Inicie PowerShell escribiendo **powershell.exe** en un símbolo del sistema.

6. Monte la ISO FoD mediante el comando siguiente:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. Tipo **salir** para salir de PowerShell.

8.  Ejecute el siguiente comando:

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  Una vez finalizada la barra de progreso, reinicie el sistema operativo.

### <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Para agregar, opcionalmente, Internet Explorer 11 a Server Core (después de agregar los DU de compatibilidad de Server Core App)

 >[!NOTE]  
   > Los DU de compatibilidad de Server Core App es necesario para la adición de Internet Explorer 11, pero no es necesario para agregar los DU de compatibilidad de Server Core aplicación Internet Explorer 11.

1.  Inicie sesión como administrador en el equipo de Server Core que tiene los DU de compatibilidad de aplicación ya ha agregado y el paquete opcional Server du que ISO se copia localmente.

2.  Inicie PowerShell escribiendo **powershell.exe** en un símbolo del sistema.

3.  Monte la ISO FoD mediante el comando siguiente:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  Tipo **salir** para salir de PowerShell.


5.  Ejecute el siguiente comando:

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  Una vez finalizada la barra de progreso, reinicie el sistema operativo.

 
#### <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notas de versiones y sugerencias para el paquete opcional du de compatibilidad de aplicación de Server Core e Internet Explorer 11

- **Importante:** , lea las notas de la versión de Windows Server 2019 algún problema, las consideraciones, o instrucciones antes de continuar con la instalación y uso del paquete opcional du de compatibilidad de aplicación de Server Core e Internet Explorer 11.
 
 >[!NOTE] 
   > Es posible que encuentre el parpadeo con la experiencia de consola de Server Core al agregar los DU de compatibilidad de la aplicación después de usar Windows Update para instalar actualizaciones acumulativas.  Este problema se resuelve con de diciembre de 2018 se actualiza.  Para obtener más información y pasos, consulte [4481610 del artículo de Knowledge Base: Pantalla parpadea después de instalar en Server Core de Windows Server 2019 du de compatibilidad de Server Core App](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Después de la instalación de la aplicación compatibilidad du y reiniciar el servidor, el color del marco de ventana de comandos consola cambiará a un tono azul diferentes.

- Si decide instalar también el paquete opcional de Internet Explorer 11, tenga en cuenta que no se admite hacer doble clic para abrir archivos .htm guardado localmente. Sin embargo, puede **haga** y elija **abrir con IE**, o bien puede abrir directamente desde Internet Explorer **archivo** -> **abrir**. 

- Para mejorar la compatibilidad de aplicaciones de Server Core con los DU de compatibilidad de aplicaciones, la consola de administración de IIS se ha agregado a Server Core como un componente opcional.  Sin embargo, es absolutamente necesario en primer lugar, agregue los DU de compatibilidad de aplicaciones para usar la consola de administración de IIS. Consola de administración de IIS se basa en Microsoft Management Console (mmc.exe), que solo está disponible en Server Core con la adición de los DU de compatibilidad de la aplicación.  Usar Powershell [ **Install-WindowsFeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) para agregar la consola de administración de IIS.

- Como un punto general de orientación, al instalar aplicaciones en Server Core (con o sin estos paquetes opcionales) a veces es necesario usar las instrucciones y opciones de instalación silenciosa. 
    
 - Por ejemplo, SQL Server Management Studio para SQL Server 2016 y SQL Server 2017 se puede instalar en Server Core y es totalmente funcional cuando los DU de compatibilidad de la aplicación está presente.  Ver, [instalar SQL Server desde el símbolo del sistema](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Si SQL Server Management Studio no es el deseado, no es necesario instalar los DU de compatibilidad de Server Core App.  Ver, [instalar SQL Server en Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

