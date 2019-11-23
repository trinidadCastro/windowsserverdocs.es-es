---
title: Crear una máquina virtual en Hyper-V
description: Proporciona instrucciones para crear una máquina virtual mediante el administrador de Hyper-V o Windows PowerShell.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 739691650ce3cda8066e9f7ac77626f53f22affa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364249"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>Crear una máquina virtual en Hyper-V

>Se aplica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Obtenga información sobre cómo crear una máquina virtual mediante el administrador de Hyper-V y Windows PowerShell y qué opciones tiene al crear una máquina virtual en el administrador de Hyper-V.  

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>Crear una máquina virtual mediante el administrador de Hyper-V  

1.  Abra **el administrador de Hyper-V**.  

2.  En el panel **acción** , haga clic en **nuevo**y, a continuación, haga clic en **máquina virtual**.  

3.  En el **Asistente para crear nueva máquina virtual**, haga clic en **siguiente**.  

4.  Realice las elecciones adecuadas para la máquina virtual en cada una de las páginas. Para obtener más información, vea [nuevas opciones de máquina virtual y valores predeterminados en el administrador de Hyper-V](#options-in-hyper-v-manager-new-virtual-machine-wizard) más adelante en este tema.  

5.  Después de comprobar las opciones en la página **Resumen** , haga clic en **Finalizar**.  

6.  En el administrador de Hyper-V, haga clic con el botón derecho en la máquina virtual y seleccione **conectar**.  

7.  En la ventana conexión de máquina virtual, seleccione **acción** > **iniciar**.  

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>Creación de una máquina virtual mediante Windows PowerShell  

1. En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.  

2. Haga clic con el botón derecho en **Windows PowerShell** y seleccione **Ejecutar como administrador**.  

3. Obtenga el nombre del conmutador virtual que desea que use la máquina virtual con [Get-VMSwitch](https://technet.microsoft.com/library/hh848499.aspx).  Por ejemplo:  

   ```  
   Get-VMSwitch  * | Format-Table Name  
   ```  

4. Use el cmdlet [New-VM](https://technet.microsoft.com/library/hh848537.aspx) para crear la máquina virtual.  Vea los ejemplos siguientes:  

   > [!NOTE]  
   > Si puede trasladar esta máquina virtual a un host de Hyper-V que ejecuta Windows Server 2012 R2, use el parámetro-version con [New-VM](https://technet.microsoft.com/library/hh848537.aspx) para establecer la versión de configuración de la máquina virtual en 5. La versión de configuración de máquina virtual predeterminada para Windows Server 2016 no es compatible con Windows Server 2012 R2 o versiones anteriores. No se puede cambiar la versión de configuración de la máquina virtual después de crear la máquina virtual. Para obtener más información, consulte [versiones de configuración de máquina virtual admitidas](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions).  

   - **Disco duro virtual existente** : para crear una máquina virtual con un disco duro virtual existente, puede usar el siguiente comando, donde,  
     - **-Name** es el nombre que se proporciona para la máquina virtual que se va a crear.  
     - **-MemoryStartupBytes** es la cantidad de memoria que está disponible para la máquina virtual al iniciarse.  
     - **-BootDevice** es el dispositivo en el que se arranca la máquina virtual cuando se inicia como el adaptador de red (adaptador de red) o el disco duro virtual (VHD).  
     - **-VHDPath** es la ruta de acceso al disco de la máquina virtual que desea usar.  
     - **-Path** es la ruta de acceso para almacenar los archivos de configuración de la máquina virtual.  
     - **-La generación** es la generación de la máquina virtual. Use la generación 1 para VHD y la generación 2 para VHDX. Consulte ¿ [debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
     - **-Switch** es el nombre del conmutador virtual que desea que la máquina virtual use para conectarse a otras máquinas virtuales o a la red. Consulte [crear un conmutador virtual para máquinas virtuales de Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  

       ```  
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
       ```  

       Por ejemplo:  

       ```  
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
       ```  

       Esto crea una máquina virtual de generación 2 denominada Win10VM con 4 GB de memoria. Se inicia desde la carpeta VMs\Win10.vhdx en el directorio actual y usa el conmutador virtual denominado ExternalSwitch. Los archivos de configuración de la máquina virtual se almacenan en la carpeta VMData.  

   - **Nuevo disco duro virtual** : para crear una máquina virtual con un nuevo disco duro virtual, reemplace el parámetro **-VHDPath** del ejemplo anterior por **-NewVHDPath** y agregue el parámetro **-NewVHDSizeBytes** . Por ejemplo:  

     ```  
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
     ```  

   - **Nuevo disco duro virtual que arranca en la imagen de sistema operativo** : para crear una máquina virtual con un nuevo disco virtual que arranque en una imagen de sistema operativo, consulte el ejemplo de PowerShell en el tutorial de creación de una [máquina virtual para Hyper-V en Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm).  

5. Inicie la máquina virtual con el cmdlet [Start-VM](https://technet.microsoft.com/library/hh848589.aspx) . Ejecute el siguiente cmdlet, donde nombre es el nombre de la máquina virtual que creó.  

   ```  
   Start-VM -Name <Name>  
   ```  

   Por ejemplo:  

   ```  
   Start-VM -Name Win10VM  
   ```  

6. Conéctese a la máquina virtual mediante la conexión de máquina virtual (VMConnect).  

   ```  
   VMConnect.exe  
   ```  

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>Opciones del Asistente para crear nueva máquina virtual del administrador de Hyper-V  
En la tabla siguiente se enumeran las opciones que puede elegir al crear una máquina virtual en el administrador de Hyper-V y los valores predeterminados para cada una.  

|Página|Predeterminado para Windows Server 2016 y Windows 10|Otras opciones|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**Especificar el nombre y la ubicación**|Nombre: nueva máquina virtual.<br /><br />Location: **C:\ProgramData\Microsoft\Windows\Hyper-V\\** .|También puede escribir su propio nombre y elegir otra ubicación para la máquina virtual.<br /><br />Aquí es donde se almacenarán los archivos de configuración de la máquina virtual.|  
|**Especificar la generación**|Generación 1|También puede crear una máquina virtual de generación 2. Para obtener más información, consulte ¿ [debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**Asignar memoria**|Memoria de Inicio: 1024 MB<br /><br />Memoria dinámica: **no seleccionado**|Puede establecer la memoria de inicio de 32 MB a 5902MB.<br /><br />También puede optar por usar Memoria dinámica. Para obtener más información, vea [información general sobre memoria dinámica de Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).|  
|**Configurar redes**|No conectado|Puede seleccionar una conexión de red para la máquina virtual que se va a usar en una lista de conmutadores virtuales existentes. Consulte [crear un conmutador virtual para máquinas virtuales de Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).|  
|**Conectar disco duro virtual**|Crear un disco duro virtual<br /><br />Nombre: <*vmname*>. vhdx<br /><br />**Ubicación**: **discos duros de C:\Users\Public\Documents\Hyper-V\Virtual\\**<br /><br />**Tamaño**: 127 GB|También puede optar por usar un disco duro virtual existente o esperar y conectar un disco duro virtual más adelante.|  
|**Opciones de instalación**|Instalar un sistema operativo más adelante|Estas opciones cambian el orden de arranque de la máquina virtual para que pueda instalar desde un archivo. ISO, un disquete de arranque o un servicio de instalación de red, como servicios de implementación de Windows (WDS).|  
|**Resumen**|Muestra las opciones que ha elegido, de modo que pueda comprobar que son correctas.<br /><br />-Nombre<br />-Generación<br />-Memoria<br />-Red<br />-Disco duro<br />-Sistema operativo|**Sugerencia:** Puede copiar el Resumen de la página y pegarlo en el correo electrónico o en otra parte para ayudarle a realizar un seguimiento de las máquinas virtuales.|  

## <a name="see-also"></a>Consulte también  

- [Nueva máquina virtual](https://technet.microsoft.com/library/hh848537.aspx)  

- [Versiones de configuración de máquina virtual admitidas](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)  

-   [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [Crear un conmutador virtual para las máquinas virtuales de Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  
