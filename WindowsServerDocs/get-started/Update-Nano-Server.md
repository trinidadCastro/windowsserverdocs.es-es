---
title: Actualización del servidor Nano
description: " "
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 0fd21f5c38968a303169b99186effd3d66f1e69b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280297"
---
# <a name="updating-nano-server"></a>Actualización del servidor Nano

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consulte [Cambios en Nano Server](nano-in-semi-annual-channel.md) para más información. 

El servidor Nano ofrece una variedad de métodos para mantenerse al día. En comparación con otras opciones de instalación de Windows Server, el servidor de Nano sigue un modelo de mantenimiento más activo parecido al de Windows 10. Estas versiones periódicas se conocen como **Rama actual para empresas (CBB)** . Este enfoque es compatible con los clientes que deseen innovar más rápidamente y pasar a una "cadencia en la nube" de ciclos de vida de desarrollo rápido. Puedes obtener más información acerca de la CBB en el [Blog de Windows Server](https://blogs.technet.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/).

**Entre estas versiones de CBB**, el servidor Nano se mantiene actualizado con una serie de *actualizaciones acumulativas*. Por ejemplo, la primera actualización acumulativa de servidor Nano se lanzó el 26 de septiembre de 2016 con [KB4093120](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120). Con esta y posteriores actualizaciones acumulativas, proporcionamos varias opciones para instalar estas actualizaciones en el servidor Nano. En este artículo, usaremos la actualización KB3192366 como ejemplo para ilustrar cómo obtener y aplicar actualizaciones acumulativas al servidor Nano. Para obtener más información sobre el modelo de actualización acumulativa, consulta el [Blog de Microsoft Update](https://blogs.technet.microsoft.com/mu/2016/10/25/patching-with-windows-server-2016/).

> [!NOTE]
> Si instalas un paquete opcional del servidor Nano desde un repositorio en línea o multimedia, este no incluirá las recientes revisiones de seguridad. Para evitar errores de coincidencia entre los paquetes opcionales y el sistema operativo base, te recomendamos que instales la última actualización acumulativa inmediatamente después de instalar los paquetes opcionales y **antes** de reiniciar el servidor.

En el caso de la actualización acumulativa para Windows 2016: 26 de septiembre de 2016 ([KB3192366](https://support.microsoft.com/en-us/kb/3192366)), debes instalar primero la actualización de la pila de mantenimiento para Windows 10 versión 1607: 23 de agosto de 2016 como requisito previo ([KB3176936](https://support.microsoft.com/en-us/kb/3176936)). Para la mayoría de las opciones siguientes, necesitas los archivos .msu que contienen los paquetes de actualización .cab. Visita el Catálogo de Microsoft Update para descargar cada uno de estos paquetes de actualización:
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366)
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936)

Después de descargar los archivos .msu el Catálogo de Microsoft Update, guardarlos en un recurso compartido de red o en un directorio local como, por ejemplo, C:\ServicingPackages. Puede cambiar el nombre de los archivos .msu según su número de KB como hemos hecho a continuación para que resulten más fáciles de identificar. A continuación, use la utilidad EXPANDIR para extraer los archivos .cab de los archivos .msu en directorios diferentes y copiar los .cabs en una sola carpeta.

```code
    mkdir C:\ServicingPackages_expanded
    mkdir C:\ServicingPackages_expanded\KB3176936
    mkdir C:\ServicingPackages_expanded\KB3192366
    Expand C:\ServicingPackages\KB3176936.msu -F:* C:\ServicingPackages_expanded\KB3176936
    Expand C:\ServicingPackages\KB3192366.msu -F:* C:\ServicingPackages_expanded\KB3192366
    mkdir C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3176936\Windows10.0-KB3176936-x64.cab C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3192366\Windows10.0-KB3192366-x64.cab C:\ServicingPackages_cabs
```

Ahora puedes usar los archivos .cab extraídos para aplicar las actualizaciones a una imagen del servidor Nano en algunas formas diferentes, según tus necesidades. Las siguientes opciones se presentan sin un orden particular de preferencia: usa la opción que tenga más sentido para tu entorno.

> [!NOTE]
> Cuando uses las herramientas DISM para el mantenimiento del servidor Nano, la versión de DISM que uses debe ser la misma o más reciente que la versión del servidor Nano para el que estés realizando el mantenimiento. Puedes hacerlo mediante la ejecución de DISM desde una versión coincidente de Windows, instalando una versión coincidente de [Windows Asssessment and Deployment Kit (ADK)](https://developer.microsoft.com/en-us/windows/hardware/windows-assessment-deployment-kit) o ejecutando DISM en el propio servidor Nano.

## <a name="option-1-integrate-a-cumulative-update-into-a-new-image"></a>Opción 1: Integrar una actualización acumulativa en una imagen nueva
Si vas a crear una nueva imagen del servidor Nano, puedes integrar la última actualización acumulativa directamente en la imagen para que se revise completamente en el primer arranque.

```powershell
New-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -<other parameters>
```

## <a name="option-2-integrate-a-cumulative-update-into-an-existing-image"></a>Opción 2: Integrar una actualización acumulativa en una imagen existente
Si tienes una imagen existente del servidor Nano que usas como punto de partida para la creación de instancias específicas del servidor Nano, puedes integrar la última actualización acumulativa directamente en la imagen de referencia existente para que las máquinas creadas con la imagen se revisen completamente en el primer arranque.

```powershell
Edit-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -TargetPath .\NanoServer.wim
```

## <a name="option-3-apply-the-cumulative-update-to-an-existing-offline-vhd-or-vhdx"></a>Opción 3: Aplicar la actualización acumulativa a un archivo sin conexión VHD o VHDX
Si tienes un disco duro virtual existente (VHD o VHDX), puedes usar las herramientas DISM para aplicar la actualización en el disco duro virtual. Debes asegurarte de que el disco no esté en uso, ya sea apagando las máquinas virtuales que estén usando el disco o desmontando el archivo de disco duro virtual.

- Con PowerShell

   ```powershell
   Mount-WindowsImage -ImagePath .\NanoServer.vhdx -Path .\MountDir -Index 1
   Add-WindowsPackage -Path .\MountDir -PackagePath  C:\ServicingPackages_cabs
   Dismount-WindowsImage -Path .\MountDir -Save
   ```

- Uso de dism.exe

   ```code
   dism.exe /Mount-Image /ImageFile:C:\NanoServer.vhdx /Index:1 /MountDir:C:\MountDir
   dism.exe /Image:C:\MountDir /Add-Package /PackagePath:C:\ServicingPackages_cabs
   dism.exe /Unmount-Image /MountDir:C:\MountDir /Commit
   ```

## <a name="option-4-apply-the-cumulative-update-to-a-running-nano-server"></a>Opción 4: Aplicar la actualización acumulativa a un servidor Nano en ejecución
Si tienes una máquina virtual del servidor Nano en ejecución o un host físico y has descargado el archivo .cab para la actualización, puedes usar las herramientas DISM para aplicar la actualización mientras el sistema operativo está conectado. Debes copiar el archivo .cab localmente en el servidor Nano o en una ubicación de red accesible. Si vas a aplicar una actualización de la pila de mantenimiento, asegúrate de que se reinicie el servidor después de aplicar la actualización de la pila de mantenimiento antes de aplicar actualizaciones adicionales.

> [!NOTE]
> Si has creado la imagen VHD o VHDX del servidor Nano mediante el cmdlet New-NanoServerImage y no has especificado un tamaño máximo para el archivo de disco duro virtual, el tamaño predeterminado de 4 GB es demasiado pequeño para aplicar la actualización acumulativa. Antes de instalar la actualización, usa el Administrador de Hyper-V, la Administración de discos, PowerShell u otra herramienta para aumentar el tamaño del disco duro virtual y el volumen del sistema como mínimo hasta 10 GB, o usa el parámetro ScratchDir en las herramientas DISM para establecer el directorio temporal en un volumen que tenga como mínimo 10 GB de espacio libre.

```powershell
$s = New-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
Copy-Item -ToSession $s -Path C:\ServicingPackages_cabs -Destination C:\ServicingPackages_cabs -Recurse
Enter-PSSession $s
```

- Con PowerShell

   ```powershell
   # Apply the servicing stack update first and then restart
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

- Uso de dism.exe
   ```powershell
   # Apply the servicing stack update first and then restart
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   
   # After the operation completes successfully and you are prompted to restart, it's safe to
   # press Ctrl+C to cancel the pipeline and return to the prompt
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

## <a name="option-5-download-and-install-the-cumulative-update-to-a-running-nano-server"></a>Opción 5: Descargar e instalar la actualización acumulativa a un servidor Nano en ejecución

Si tienes una máquina virtual del servidor Nano en ejecución o un host físico, puedes usar el proveedor de WMI de Windows Update para descargar e instalar la actualización mientras el sistema operativo está conectado. Con este método, no necesitas descargar el archivo .msu por separado desde el Catálogo de Microsoft Update. El proveedor de WMI detectará, descargará e instalará todas las actualizaciones disponibles al mismo tiempo.

```powershell
Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
```

- Buscar actualizaciones disponibles
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}
   $result.Updates
   ```

- Instalar todas las actualizaciones disponibles
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   Invoke-CimMethod -InputObject $ci -MethodName ApplyApplicableUpdates
   Restart-Computer; exit
   ```

- Obtener una lista de actualizaciones instaladas
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}
   $result.Updates
   ```
   
## <a name="additional-options"></a>Opciones adicionales
Es posible que otros métodos para actualizar el servidor Nano superpongan o complementen las opciones anteriores. Estas opciones incluyen el uso de Windows Server Update Services (WSUS), System Center Virtual Machine Manager (VMM), el programador de tareas o una solución de terceros.
- [Configurar Windows Update para WSUS](https://msdn.microsoft.com/library/dd939844(v=ws.10).aspx) mediante las siguientes claves del Registro:
  - WUServer
  - WUStatusServer (por lo general, usa el mismo valor que WUServer)
  - UseWUServer
  - AUOptions
- [Administración de las actualizaciones de tejidos en VMM](https://technet.microsoft.com/library/gg675084(v=sc.12).aspx)
- [Registro de una tarea programada](https://technet.microsoft.com/library/jj649811.aspx)