---
title: Característica a petición (FOD) App Compatibility de Server Core
description: Cómo instalar las características a petición de Windows Server
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 441cd9593371cb7e5018a61c3d7a6af991ce8fed
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280166"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Característica a petición (FOD) App Compatibility de Server Core

> Se aplica a: Windows Server 2019, Canal semianual de Windows Server

La **característica a petición App Compatibility de Server Core** es un paquete opcional de características que se puede agregar a las instalaciones básicas de Windows Server 2019 o al Canal semianual de Windows Server en cualquier momento.

Para más información sobre las características a petición (FOD), consulta [Features On Demand](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) (Características a petición).

## <a name="why-install-the-app-compatibility-fod"></a>¿Por qué instalar las características a petición App Compatibility?

La característica a petición App Compatibility de Server Core mejora significativamente la compatibilidad de las aplicaciones de la opción de instalación básica de Windows Server, al incluir un subconjunto de archivos binarios y paquetes de Windows Server con la experiencia de escritorio sin agregar el entorno gráfico de la experiencia de escritorio de Windows Server. Este paquete opcional está disponible en un archivo ISO independiente, o desde Windows Update, pero solo se puede agregar a las imágenes e instalaciones básicas de Windows Server.

Los dos valores principales que proporciona la característica a petición App Compatibility son los siguientes:

- Aumenta la compatibilidad de Server Core para las aplicaciones de servidor que ya están en el mercado o que ya se han desarrollado por organizaciones y se han implementado.
- Ayuda a proporcionar componentes del sistema operativo y a aumentar la compatibilidad de las aplicaciones de las herramientas de software utilizadas en la resolución de problemas importantes y en los escenarios de depuración.

Los componentes del sistema operativo que están disponibles como parte de la característica a petición App Compatibility de Server Core incluyen:

-   Microsoft Management Console (mmc.exe)

-   Visor de eventos (Eventvwr.msc)

-   Monitor de rendimiento (PerfMon.exe)

-   Monitor de recursos (Resmon.exe)

-   Administrador de dispositivos (Devmgmt.msc)

-   Explorador de archivos (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Administración de discos (Diskmgmt.msc)

-   Administrador de clústeres de conmutación por error (CluAdmin.msc)

    -   Requiere primero la adición de la característica de clústeres de conmutación por error de Windows Server.

        -   Desde una sesión de PowerShell con privilegios elevados: 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   Para ejecutar el Administrador de clústeres de conmutación por error, escriba **cluadmin** en el símbolo del sistema.

Los servidores que ejecutan Windows Server, versión 1903 y posteriores, también admiten los siguientes componentes (cuando se utiliza la misma versión de la característica a petición App Compatibility):

- Administrador de Hyper-V (virtmgmt.msc)
- Programador de tareas (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>Instalación de la característica a petición App Compatibility

Solo se puede realizar una instalación básica de la característica a petición App Compatibility. No intentes agregar la característica a petición App Compatibility de Server Core a una instalación de Windows Server con experiencia de escritorio. Los mismos paquetes opcionales de la característica a petición pueden utilizarse para las instalaciones básicas de Windows Server 2019 o las instalaciones del Canal semianual de Windows Server.

1. Si el servidor puede conectarse a Windows Update, todo lo que tienes que hacer es ejecutar el siguiente comando desde una sesión de PowerShell con privilegios elevados y luego reiniciar Windows Server después de que el comando termine de ejecutarse:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. Si el servidor no puede conectarse a Windows Update, descarga los paquetes opcionales ISO de la característica a petición del servidor y copia la ISO en una carpeta compartida de tu red local:

   - Si tienes una licencia por volumen, puedes descargar el archivo de imagen ISO de la característica a petición de Server desde el mismo portal en el que se obtiene el archivo de imagen ISO del sistema operativo: [Centro de servicio de licencias por volumen](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - El archivo de imagen ISO de la característica a petición del servidor también está disponible en [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) o en el [portal de Visual Studio](https://visualstudio.microsoft.com) para suscriptores.

3. Inicie sesión con una cuenta de administrador en el equipo principal que está conectado a la red local y al que desea agregar la característica a petición.

4. Utiliza **net use**, o cualquier otro método, para conectarte a la ubicación de la imagen ISO de la característica a petición.

5. Copia la imagen ISO de la característica a petición en una carpeta local de tu elección.

6. Monta la imagen ISO de la característica a petición mediante el siguiente comando en una sesión de PowerShell con privilegios elevados:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. Ejecuta el siguiente comando:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. Una vez finalizada la barra de progreso, reinicia el sistema operativo.

   Para más información acerca de los comandos DISM, consulta [Use DISM in Windows PowerShell](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14) (Uso de DISM en Windows PowerShell).

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Para agregar opcionalmente Internet Explorer 11 a Server Core (después de agregar la característica a petición App Compatibility de Server Core)

 >[!NOTE]  
   > La característica a petición App Compatibility de Server Core es necesaria para agregar Internet Explorer 11, pero Internet Explorer 11 no es necesario para agregar la característica a petición App Compatibility de Server Core.

1. Inicia sesión como administrador en el equipo de Server Core que tiene la característica a petición App Compatibility ya agregada y la imagen ISO del paquete opcional de la característica a petición App Compatibility copiada localmente.

2. Para iniciar PowerShell, escribe **powershell.exe** en un símbolo del sistema.

3. Monta la imagen ISO de la característica a petición con el comando siguiente:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. Ejecuta el siguiente comando mediante la variable `$package_path` para especificar la ruta de acceso del archivo cab de Internet Explorer:

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. Una vez finalizada la barra de progreso, reinicia el sistema operativo.

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notas de la versión y sugerencias para el paquete opcional de la característica a petición App Compatibility de Server Core e Internet Explorer 11

> [!IMPORTANT]
> Las características a petición instaladas en Windows Server, versión 1809 no permanecerán en su lugar después de una actualización a Windows Server, versión 1903, por lo que tendrá que instalarlas de nuevo después de la actualización. Como alternativa, puede agregar características a petición al nuevo origen de la instalación de Windows Server antes de realizar la actualización. Esto garantiza que la nueva versión de las características a petición está presente cuando se completa la actualización. Para más información, consulta la sección [Adición de funcionalidades y paquetes opcionales a una imagen de Server Core de WIM sin conexión](install-fod-19.md#add-capabilities).

- **Importante:** Lee las notas de la versión de Windows Server 2019 para conocer cualquier problema, consideración o guía antes de continuar con la instalación y el uso del paquete opcional de la característica a petición App Compatibility de Server Core e Internet Explorer 11.

- Es posible que veas un parpadeo con la experiencia de la consola de Server Core al agregar la característica a petición App Compatibility después de usar Windows Update para instalar las actualizaciones acumulativas.  Este problema se ha resuelto con las actualizaciones de diciembre de 2018.  Para tener más información y conocer los pasos de resolución, consulta [el artículo 4481610 de Knowledge Base: Screen flickers after you install Server Core App Compatibility FOD in Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core) (La pantalla parpadea después de instalar la característica a petición App Compatibility de Server Core en Windows Server 2019).

- Después de la instalación de la característica a petición App Compatibility y del reinicio del servidor, el color del marco de la ventana de la consola de comandos cambia a un tono de azul diferente.

- Si decides instalar también el paquete opcional de Internet Explorer 11, ten en cuenta que no es posible hacer doble clic para abrir los archivos .htm guardados localmente. Sin embargo, puedes **hacer clic con el botón derecho** y elegir **Abrir con IE**, o bien puedes abrirlo directamente desde Internet Explorer **Archivo** -> **Abrir**.

- Para mejorar aún más la compatibilidad de las aplicaciones de Server Core con la característica a petición App Compatibility, la consola de administración de IIS se ha agregado a Server Core como componente opcional.  Sin embargo, es absolutamente necesario agregar primero la característica a petición App Compatibility para utilizar la consola de administración de IIS. La consola de administración de IIS se basa en Microsoft Management Console (mmc.exe), que solo está disponible en Server Core con la adición de la característica a petición App Compatibility.  Utiliza [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) de Powershell para agregar la consola de administración de IIS.

- Como guía general, cuando se instalan aplicaciones en Server Core (con o sin estos paquetes opcionales), a veces es necesario utilizar opciones e instrucciones de instalación silenciosa. 
    
  - Por ejemplo, SQL Server Management Studio para SQL Server 2016 y SQL Server 2017 pueden instalarse en Server Core y son totalmente funcionales cuando está presente la característica a petición App Compatibility.  Consulta [Instalar SQL Server desde el símbolo del sistema](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
  - Si SQL Server Management Studio no se desea, no es necesario instalar la característica a petición App Compatibility de Server Core.  Consulta [Instalar SQL Server en Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities">Adición de funcionalidades y paquetes opcionales a una imagen de Server Core de WIM sin conexión

1. Descarga los archivos de imagen ISO de la característica a petición de Server y de Windows Server en una carpeta local en un equipo Windows.

   - Si tienes una licencia por volumen, puedes descargar los archivos de imagen ISO de la característica a petición de Server y de Windows Server en el [Centro de servicio de licencias por volumen](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - El archivo de imagen ISO de la característica a petición del servidor también está disponible para las versiones del Canal de mantenimiento a largo plazo en [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) o en el [portal de Visual Studio](https://visualstudio.microsoft.com) para suscriptores.

2. Abre una sesión de PowerShell como administrador y utiliza después los siguientes comandos para montar los archivos de imagen como unidades:

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. Copia el contenido del archivo ISO de Windows Server en una carpeta local (por ejemplo, *C:\SetupFiles\WindowsServer*).

4. Consigue el nombre de la imagen que deseas modificar en el archivo Install.wim mediante el comando siguiente.<br>
Utiliza la variable `$install_wim_path` para especificar la ruta de acceso al archivo Install.wim, se encuentra dentro de la carpeta \Sources del archivo ISO.

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. Monta el archivo Install.wim en una nueva carpeta con el siguiente comando reemplazando los valores de la variable de ejemplo con los tuyos propios, y reutilizando la variable `$install_wim_path` del comando anterior.<br>

   - `$image_name`: escribe el nombre de la imagen que quieres montar.
   - `$mount_folder variable`: especifica la carpeta que usarás al tener acceso al contenido del archivo Install.wim.

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. Agrega las funcionalidades y los paquetes que quieras a la imagen Install.wim montada mediante los siguientes comandos, sustituyendo los valores de las variables de ejemplo por los tuyos propios.<br>

   - `$capability_name`: especifica el nombre de la funcionalidad que quieres instalar (en este caso, la funcionalidad AppCompatibility).
   - `$package_path`: especifica la ruta de acceso al paquete de instalación (en este caso, Internet Explorer).
   - `$fod_drive`: especifica la letra de unidad de la imagen de la característica a petición del servidor montado.

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. Desmonta y confirma los cambios en el archivo Install.wim mediante el comando siguiente, que utiliza la variable `$mount_folder` de los comandos anteriores:

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

Ahora puedes actualizar el servidor mediante la ejecución de setup.exe en la carpeta que has creado para los archivos de instalación de Windows Server (en este ejemplo: *C:\SetupFiles\WindowsServer*). Ahora, esta carpeta contiene los archivos de instalación de Windows Server con las funcionalidades adicionales y paquetes opcionales incluidos.