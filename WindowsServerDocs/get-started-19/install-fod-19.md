---
title: Característica de compatibilidad de aplicación de Server Core a petición (FOD)
description: Cómo instalar características de Windows Server a petición
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 747258601aa05885d209aacde6947eb7b05e8121
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810795"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Característica de compatibilidad de aplicación de Server Core a petición (FOD)

> Se aplica a: Windows Server 2019, canal semianual de Windows Server

El **característica de compatibilidad de Server Core App petición** es un paquete de la característica opcional que puede agregarse a las instalaciones Server Core de Windows Server 2019 o canal semianual de Windows Server, en cualquier momento.

Para obtener más información sobre las características en la demanda (du), consulte [características a petición](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

## <a name="why-install-the-app-compatibility-fod"></a>¿Por qué instalar los DU de compatibilidad de la aplicación?

Compatibilidad de aplicaciones, una característica a petición para Server Core, mejora significativamente la compatibilidad de aplicaciones de la opción de instalación Server Core de Windows mediante la inclusión de un subconjunto de los archivos binarios y paquetes de Windows Server con experiencia de escritorio, sin agregar el Entorno gráfico de la experiencia de escritorio de Windows Server. Este paquete opcional está disponible en un archivo ISO independiente, o desde Windows Update, pero solo puede agregarse a las imágenes y las instalaciones Server Core de Windows.

Los dos valores principales que proporciona los DU de compatibilidad de aplicación son:

- Aumenta la compatibilidad de Server Core para aplicaciones de servidor que ya están en el mercado o ya se ha desarrollado por organizaciones e implementado.
- Ayuda con la prestación de los componentes del sistema operativo y aplicación una mayor compatibilidad de herramientas de software usado aguda para solucionar problemas y escenarios de depuración.

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

        -   Desde una sesión de PowerShell con privilegios elevados: 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   Para ejecutar el Administrador de clústeres de conmutación por error, escriba **cluadmin** en el símbolo del sistema.

Los servidores que ejecutan Windows Server, versión 1903 y versiones posterior también admiten los siguientes componentes (cuando se usa la misma versión de los DU de compatibilidad de aplicación):

- Hyper-V Manager (virtmgmt.msc)
- Programador de tareas (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>Instalar la compatibilidad de aplicaciones du

Los DU de compatibilidad de la aplicación solo puede instalarse en Server Core. No intente agregar los DU de compatibilidad de Server Core App a una instalación de Windows Server de Windows Server con experiencia de escritorio. Los mismos paquetes opcionales de du ISO pueden utilizarse para las instalaciones Server Core de Windows Server 2019 o las instalaciones de canal semianual de Windows Server.

1. Si el servidor puede conectarse a Windows Update, lo único que debe hacer es ejecutar el siguiente comando desde una sesión de PowerShell con privilegios elevados y, a continuación, reinicie Windows Server después de que el comando termine de ejecutarse:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. Si el servidor no puede conectarse a Windows Update, en su lugar descargar los paquetes opcionales del servidor du ISO y copie la imagen ISO a una carpeta compartida en la red local:

   - Si tiene una licencia por volumen, que puede descargar el archivo de imagen ISO de du Server desde el mismo portal donde se obtiene el archivo de imagen ISO del sistema operativo: [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - El archivo de imagen ISO de du Server también está disponible en el [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) o en el [portal de Visual Studio](https://visualstudio.microsoft.com) para los suscriptores.

3. Inicie sesión con una cuenta de administrador en el equipo que está conectado a la red local y que desea agregar los DU para Server Core.

4. Usar **net use**, o algún otro método para conectarse a la ubicación de la ISO FOD.

5. Copie FOD ISO en una carpeta local de su elección.

6. Monte FOD ISO mediante el siguiente comando en una sesión de PowerShell con privilegios elevados:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. Ejecute el siguiente comando:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. Una vez finalizada la barra de progreso, reinicie el sistema operativo.

   Para obtener más información acerca de los comandos DISM, vea [usar DISM en Windows PowerShell](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Para agregar, opcionalmente, Internet Explorer 11 a Server Core (después de agregar los DU de compatibilidad de Server Core App)

 >[!NOTE]  
   > Los DU de compatibilidad de Server Core App es necesario para la adición de Internet Explorer 11, pero no es necesario para agregar los DU de compatibilidad de Server Core aplicación Internet Explorer 11.

1. Inicie sesión como administrador en el equipo de Server Core que tiene los DU de compatibilidad de aplicación ya ha agregado y el paquete opcional Server du que ISO se copia localmente.

2. Inicie PowerShell escribiendo **powershell.exe** en un símbolo del sistema.

3. Monte la ISO FOD mediante el comando siguiente:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. Ejecute el siguiente comando, use el `$package_path` variable para especificar la ruta de acceso del archivo cab de Internet Explorer:

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. Una vez finalizada la barra de progreso, reinicie el sistema operativo.

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notas de versiones y sugerencias para el paquete opcional du de compatibilidad de aplicación de Server Core e Internet Explorer 11

> [!IMPORTANT]
> FODs instalados en Windows Server, versión 1809 no sigue estando en su lugar después de una actualización en contexto a Windows Server, versión 1903, tendría que instalarlos de nuevo después de la actualización. Como alternativa, puede agregar FODs al nuevo origen de instalación de Windows Server antes de actualizar. Esto garantiza que la nueva versión de cualquier FODs están presentes cuando se completa la actualización. Para obtener más información, consulte el [agregar funciones y paquetes opcionales a una imagen de Server Core de WIM sin conexión](install-fod-19.md#add-capabilities).

- **Importante:** Lea las notas de la versión de Windows Server 2019 algún problema, las consideraciones, o instrucciones antes de continuar con la instalación y uso del paquete opcional du de compatibilidad de aplicación de Server Core e Internet Explorer 11.

- Es posible que encuentre el parpadeo con la experiencia de consola de Server Core al agregar los DU de compatibilidad de la aplicación después de usar Windows Update para instalar actualizaciones acumulativas.  Este problema se resuelve con de diciembre de 2018 se actualiza.  Para obtener más información y pasos, consulte [4481610 del artículo de Knowledge Base: Pantalla parpadea después de instalar en Server Core de Windows Server 2019 du de compatibilidad de Server Core App](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Después de la instalación de la aplicación compatibilidad du y reiniciar el servidor, el color del marco de ventana de comandos consola cambiará a un tono azul diferentes.

- Si decide instalar también el paquete opcional de Internet Explorer 11, tenga en cuenta que no se admite hacer doble clic para abrir archivos .htm guardado localmente. Sin embargo, puede **haga** y elija **abrir con IE**, o bien puede abrir directamente desde Internet Explorer **archivo** -> **abrir**.

- Para mejorar la compatibilidad de aplicaciones de Server Core con los DU de compatibilidad de aplicaciones, la consola de administración de IIS se ha agregado a Server Core como un componente opcional.  Sin embargo, es absolutamente necesario en primer lugar, agregue los DU de compatibilidad de aplicaciones para usar la consola de administración de IIS. Consola de administración de IIS se basa en Microsoft Management Console (mmc.exe), que solo está disponible en Server Core con la adición de los DU de compatibilidad de la aplicación.  Usar Powershell [ **Install-WindowsFeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) para agregar la consola de administración de IIS.

- Como un punto general de orientación, al instalar aplicaciones en Server Core (con o sin estos paquetes opcionales) a veces es necesario usar las instrucciones y opciones de instalación silenciosa. 
    
  - Por ejemplo, SQL Server Management Studio para SQL Server 2016 y SQL Server 2017 se puede instalar en Server Core y es totalmente funcional cuando los DU de compatibilidad de la aplicación está presente.  Ver, [instalar SQL Server desde el símbolo del sistema](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
  - Si SQL Server Management Studio no es el deseado, no es necesario instalar los DU de compatibilidad de Server Core App.  Ver, [instalar SQL Server en Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> Agregar funciones y paquetes opcionales a una imagen de Server Core de WIM sin conexión

1. Descargue los archivos de imagen de Windows Server y servidor du ISO en una carpeta local en un equipo Windows.

   - Si tiene una licencia por volumen, puede descargar los archivos de imagen de Windows Server y servidor du ISO de la [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - El archivo de imagen ISO de du Server también está disponible para las versiones de canal de servicio a largo plazo en el [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) o en el [portal de Visual Studio](https://visualstudio.microsoft.com) para los suscriptores.

2. Abra una sesión de PowerShell como administrador y, a continuación, use los siguientes comandos para montar los archivos de imagen como unidades de disco:

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. Copia el contenido del archivo ISO de Windows Server en una carpeta local (por ejemplo, *C:\SetupFiles\WindowsServer*).

4. Obtiene el nombre de la imagen que desea modificar en el archivo Install.wim mediante el comando siguiente.<br>
Use el `$install_wim_path` variable para especificar la ruta de acceso al archivo Install.wim, se encuentra dentro de la carpeta \Sources del archivo ISO.

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. Montar el archivo Install.wim en una carpeta nueva con el siguiente comando, reemplazando los valores de variable del ejemplo por los suyos propios y volver a usar el `$install_wim_path` variable por el comando anterior.<br>

   - `$image_name` -Escriba el nombre de la imagen que desea montar.
   - `$mount_folder variable` -Especifique la carpeta que se usará al tener acceso al contenido del archivo Install.wim.

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. Agregar capacidades y los paquetes que desee a la imagen Install.wim montada mediante los comandos siguientes, reemplazando los valores de variable del ejemplo por los suyos propios.<br>

   - `$capability_name` -Especifique el nombre de la capacidad para instalar (en este caso, la capacidad de AppCompatibility).
   - `$package_path` -Especifique la ruta de acceso al paquete de instalación (en este caso, Internet Explorer).
   - `$fod_drive` -Especifique la letra de unidad de la imagen montada de du de servidor.

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. Desmonte y confirme los cambios en el archivo Install.wim mediante el comando siguiente, que usa el `$mount_folder` variable de comandos anteriores:

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

Ahora puede actualizar el servidor ejecutando setup.exe en la carpeta que creó para los archivos de instalación de Windows Server (en este ejemplo: *C:\SetupFiles\WindowsServer*). Ahora, esta carpeta contiene los archivos de instalación de Windows Server con las funcionalidades adicionales y paquetes opcionales incluidos.