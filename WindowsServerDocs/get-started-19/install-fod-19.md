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
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149905"
---
# Característica de compatibilidad de aplicación de Server Core a petición (FOD)

> Se aplica a Windows Server 2019 y Windows Server, versión 1809

La **Función de compatibilidad de aplicaciones de Server Core a petición** es un paquete de la característica opcional que se puede agregar a las instalaciones de Server Core de Windows Server 2019, o Windows Server, versión 1809, en cualquier momento.

Para obtener más información sobre las características a petición (FOD), consulta [Características a petición](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).


## ¿Por qué instalar los DU de compatibilidad de la aplicación? 

Compatibilidad de aplicaciones, una característica a petición de Server Core, mejora significativamente la compatibilidad de aplicaciones de la opción de instalación de Windows Server Core al incluir un subconjunto de los archivos binarios y los paquetes de Windows Server con experiencia de escritorio, sin agregar el Entorno gráfico de la experiencia de escritorio de Windows Server. Este paquete opcional está disponible en una ISO independiente, o desde Windows Update, pero solo se puede agregar a imágenes e instalaciones de Windows Server Core.

Los dos valores principales que proporciona los DU de compatibilidad de la aplicación son:

1.  Aumenta la compatibilidad de Server Core para aplicaciones de servidor que ya están en el mercado o ya se han desarrollado por las organizaciones e implementado.

2.  Ayuda con lo que proporciona componentes de sistema operativo y compatibilidad de aplicaciones mayor de herramientas de software usado aguda para solucionar problemas y escenarios de depuración.

Componentes de sistema operativo que están disponibles como parte de los DU de compatibilidad de aplicación de Server Core se incluyen:

-   Microsoft Management Console (mmc.exe)

-   Visor de eventos (Eventvwr.msc)

-   Monitor de rendimiento (PerfMon.exe)

-   Monitor de recursos (Resmon.exe)

-   Administrador de dispositivos (Devmgmt.msc)

-   Explorador de archivos (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Administración de discos (Diskmgmt.msc)

-   Administrador de clústeres de conmutación por error (CluAdmin.msc)

    -   Requiere la incorporación de la característica de Windows Server de clústeres de conmutación por error en primer lugar.

        -   Usar el Cmdlet de Powershell para agregar, `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   Para ejecutar el Administrador de clústeres de conmutación por error, escribe **cluadmin** en el símbolo del sistema.

## Instalación de la compatibilidad de aplicaciones du

 >[!IMPORTANT] 
   >Los DU de compatibilidad de la aplicación solo se puede instalar en Server Core. No intentes agregar los DU de compatibilidad de aplicación de Server Core a una instalación de Windows Server de Windows Server con experiencia de escritorio.

### Para agregar la característica de compatibilidad de aplicaciones de Server Core a petición (FOD) a una instancia de Server Core

 >[!NOTE] 
   > Este procedimiento usa el mantenimiento de imágenes de implementación y administración (DISM.exe), una herramienta de línea de comandos. Para obtener más información acerca de los comandos DISM, consulta las [Opciones de línea de comandos de mantenimiento de DISM funcionalidades paquete](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options).

>[!NOTE] 
   > Los mismos paquetes opcionales de du ISO pueden usarse para las instalaciones Server Core de Windows Server 2019, o Windows Server, versión 1809, las instalaciones.

>[!NOTE] 
   > Si el equipo o máquina virtual que se ejecuta Server Core es capaz de conectarse a Windows Update, se puede omitir los pasos 1-7 a continuación. Pero Asegúrate de dejar desactivar /Source y /LimitAccess desde el comando DISM en el paso 8.

1. Descargar los paquetes opcionales de servidor du ISO y copia la imagen ISO a una carpeta compartida de la red local:

 - Si tienes una licencia por volumen puede descargar el archivo de imagen ISO de du de servidor desde el mismo portal donde se obtiene el archivo de imagen ISO de sistema operativo: [Centro de servicio de licencias por volumen](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
 - El archivo de imagen ISO de du Server también está disponible en el [Centro de evaluación de Microsoft](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) o en el [portal de Visual Studio](https://visualstudio.microsoft.com) para suscriptores.


2. Inicia sesión como administrador en el equipo que está conectado a la red local y que quieres agregar los DU a Server Core.

3. Usar **net use**o algún otro método para conectarse a la ubicación de la ISO FOD.

4. Copia el ISO FOD en una carpeta local de su elección.

5. Inicia PowerShell escribiendo **powershell.exe** en un símbolo del sistema.

6. Monta el ISO FoD mediante el siguiente comando:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. Escriba **exit** para salir de PowerShell.

8.  Ejecuta el siguiente comando:

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  Una vez completada la barra de progreso, reinicia el sistema operativo.

### Opcionalmente, agregar Internet Explorer 11 a Server Core (después de agregar los DU de compatibilidad de aplicación de Server Core)

 >[!NOTE]  
   > Los DU de compatibilidad de aplicación de Server Core es necesario para la adición de Internet Explorer 11, pero Internet Explorer 11 no es necesario agregar los DU de compatibilidad de aplicación de Server Core.

1.  Inicia sesión como administrador en el equipo de Server Core que tiene los DU de compatibilidad de la aplicación ya se ha agregado y el paquete opcional de servidor du que ISO copiado localmente.

2.  Inicia PowerShell escribiendo **powershell.exe** en un símbolo del sistema.

3.  Monta el ISO FoD mediante el siguiente comando:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  Escriba **exit** para salir de PowerShell.


5.  Ejecuta el siguiente comando:

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  Una vez completada la barra de progreso, reinicia el sistema operativo.

 
#### Notas de versiones y sugerencias para el paquete opcional du de compatibilidad de aplicación de Server Core e Internet Explorer 11

- **Importante:** lee las notas de la versión de Windows Server 2019 para los problemas, consideraciones o instrucciones antes de continuar con la instalación y el uso del paquete opcional du de compatibilidad de aplicación de Server Core e Internet Explorer 11.
 
 >[!NOTE] 
   > Es posible que surjan parpadeo con la experiencia de la consola de Server Core, al agregar los DU de compatibilidad de la aplicación después de usar Windows Update para instalar las actualizaciones acumulativas.  Este problema se resuelve con diciembre de 2018, se actualiza.  Para obtener más información y la resolución pasos, consulte [artículo de Knowledge Base 4481610: pantalla parpadea después de instalar la aplicación compatibilidad du de Server Core en Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Tras la instalación de la aplicación compatibilidad du y reiniciar el servidor, el color de marco de ventana de consola de comando cambiará a un tono diferente de azul.

- Si decides también instalar el paquete opcional de Internet Explorer 11, ten en cuenta que no se admite hacer doble clic para abrir archivos .htm guardado localmente. Sin embargo, es posible **con el botón derecho** y selecciona **Abrir con IE**, o puede abrirlo directamente desde Internet Explorer **archivo** -> **Abrir**. 

- Para mejorar la compatibilidad de aplicaciones de Server Core con los DU de compatibilidad de la aplicación, la consola de administración de IIS se ha agregado a Server Core como un componente opcional.  Sin embargo, es absolutamente necesario en primer lugar, agrega los DU de compatibilidad de la aplicación para usar la consola de administración de IIS. Consola de administración de IIS se basa en Microsoft Management Console (mmc.exe), que solo está disponible en Server Core con la adición de los DU de compatibilidad de la aplicación.  Usar Powershell [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) para agregar la consola de administración de IIS.

- Como un punto general de orientación, cuando la instalación de aplicaciones en Server Core (con o sin estos paquetes opcionales) a veces es necesario usar instrucciones y opciones de instalación silenciosa. 
    
 - Por ejemplo, SQL Server Management Studio para SQL Server 2016 y 2017 de SQL Server se puede instalar en Server Core y es completamente funcional cuando los DU de compatibilidad de la aplicación está presente.  Ver, [instalar SQL Server desde el símbolo del sistema](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Si no se desea SQL Server Management Studio, no es necesario instalar los DU de compatibilidad de aplicación de Server Core.  Ver, [instalar SQL Server en Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

