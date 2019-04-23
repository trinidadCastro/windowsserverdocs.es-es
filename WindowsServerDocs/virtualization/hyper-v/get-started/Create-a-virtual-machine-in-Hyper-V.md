---
title: Crear una máquina virtual de Hyper-V
description: Proporciona instrucciones para crear una máquina virtual mediante el Administrador de Hyper-V o Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 8bc0aba906066f1c2fdc4d906ebbc78b08e871f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852146"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>Crear una máquina virtual de Hyper-V

>Se aplica a: Windows 10, Windows Server 2016 y Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Obtenga información sobre cómo crear una máquina virtual mediante el Administrador de Hyper-V y Windows PowerShell y qué opciones tienen cuando se crea una máquina virtual en Hyper-V Manager.  

## <a name="BKMK_HyperVManager"></a>Crear una máquina virtual mediante el Administrador de Hyper-V  

1.  Abra **Administrador de Hyper-V**.  

2.  Desde el **acción** panel, haga clic en **New**y, a continuación, haga clic en **Máquina Virtual**.  

3.  Desde el **Asistente para nueva máquina Virtual**, haga clic en **siguiente**.  

4.  Tomar las decisiones adecuadas para la máquina virtual en cada una de las páginas. Para obtener más información, consulte [nuevas opciones de la máquina virtual y los valores predeterminados en el Administrador de Hyper-V](#BKMK_Options) más adelante en este tema.  

5.  Después de comprobar las opciones de la **resumen** página, haga clic en **finalizar**.  

6.  En el Administrador de Hyper-V, haga clic en la máquina virtual y seleccione **conectar**.  

7.  En la ventana conexión a máquina Virtual, seleccione **acción** > **iniciar**.  

## <a name="BKMK_PowerShell"></a>Crear una máquina virtual con Windows PowerShell  

1.  En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.  

2.  Haga clic en **Windows PowerShell** y seleccione **ejecutar como administrador**.  

3.  Obtener el nombre del conmutador virtual que desea que la máquina virtual para usar con [Get-VMSwitch](https://technet.microsoft.com/library/hh848499.aspx).  Por ejemplo,  

    ```  
    Get-VMSwitch  * | Format-Table Name  
    ```  

4.  Use la [New-VM](https://technet.microsoft.com/library/hh848537.aspx) para crear la máquina virtual.  Vea los ejemplos siguientes:  

    > [!NOTE]  
    > Si se puede mover esta máquina virtual a un host de Hyper-V que ejecute Windows Server 2012 R2, use el parámetro - Version con [New-VM](https://technet.microsoft.com/library/hh848537.aspx) para establecer la versión de configuración de máquina virtual a 5. La versión de configuración de máquina virtual predeterminada para Windows Server 2016 no es compatible con Windows Server 2012 R2 o versiones anteriores. No se puede cambiar la versión de configuración de máquina virtual una vez creada la máquina virtual. Para obtener más información, consulte [admite versiones de configuración de máquina virtual](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#BKMK_SupportedConfigVersions).  

    - **Disco duro virtual existente** : para crear una máquina virtual con un disco duro virtual existente, puede usar el siguiente comando donde,  
        -   **-Nombre** es el nombre que proporcione para la máquina virtual que se va a crear.  
        -   **-MemoryStartupBytes** es la cantidad de memoria que está disponible para la máquina virtual durante el inicio.  
        -   **-BootDevice** es el dispositivo que arranca la máquina virtual cuando se inicia como el adaptador de red (adaptador de red) o un disco duro virtual (VHD).  
        -   **-VHDPath** es la ruta de acceso en el disco de máquina virtual que desea usar.  
        -   **-Ruta de acceso** es la ruta de acceso para almacenar los archivos de configuración de máquina virtual.  
        -   **-Generación** es la generación de máquina virtual. Utilice la generación 1 para la generación 2 para VHDX y el disco duro virtual. Consulte [debo crear una máquina virtual de generación 1 o 2 en Hyper-V?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
        -   **-Cambiar** es el nombre del conmutador virtual que desea que la máquina virtual para conectarse a otras máquinas virtuales o a la red. Consulte [crear un conmutador virtual para las máquinas virtuales de Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  

        ```  
        New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
        ```  

        Por ejemplo:  

        ```  
        New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
        ```  

        Esto crea una máquina virtual de generación 2 denominada Win10VM con 4GB de memoria. Se inicia desde la carpeta VMs\Win10.vhdx en el directorio actual y usa el conmutador virtual denominado ExternalSwitch. Los archivos de configuración de máquina virtual se almacenan en la carpeta VMData.  

    -   **Nuevo disco duro virtual** : para crear una máquina virtual con un nuevo disco duro virtual, reemplace el **- VHDPath** parámetro desde el ejemplo anterior con **- NewVHDPath** y agregue el **- NewVHDSizeBytes** parámetro. Por ejemplo,  

        ```  
        New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
        ```  

    -   **Nuevo disco duro virtual que se arranca con la imagen de sistema operativo** : para crear una máquina virtual con un nuevo disco virtual que se inicia con una imagen de sistema operativo, vea el ejemplo de PowerShell de [crear tutorial de la máquina virtual de Hyper-V en Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm).  

5.  Inicie la máquina virtual mediante la [Start-VM](https://technet.microsoft.com/library/hh848589.aspx) cmdlet. Ejecute el siguiente cmdlet donde Name es el nombre de la máquina virtual que creó.  

    ```  
    Start-VM -Name <Name>  
    ```  

    Por ejemplo:  

    ```  
    Start-VM -Name Win10VM  
    ```  

6.  Conéctese a la máquina virtual con conexión a máquina Virtual (VMConnect).  

    ```  
    VMConnect.exe  
    ```  

## <a name="BKMK_Options"></a>Opciones de Asistente de máquina Virtual nueva de administrador de Hyper-V  
En la tabla siguiente se enumera las opciones que puede elegir al crear una máquina virtual en el Administrador de Hyper-V y los valores predeterminados para cada uno.  

|Página|Valor predeterminado para Windows Server 2016 y Windows 10|Otras opciones|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**Especificar el nombre y la ubicación**|Nombre:  Nueva máquina Virtual.<br /><br />Ubicación:  **C:\ProgramData\Microsoft\Windows\Hyper-V\\**.|También puede escribir su propio nombre y elegir otra ubicación para la máquina virtual.<br /><br />Esto es donde se almacenarán los archivos de configuración de máquina virtual.|  
|**Especificar la generación**|Generación 1|También puede crear una máquina virtual de generación 2. Para obtener más información, consulte [debo crear una máquina virtual de generación 1 o 2 en Hyper-V?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**Asignar memoria**|Memoria de inicio: 1024 MB<br /><br />Memoria dinámica: **no seleccionado**|Puede establecer la memoria de inicio de 32MB a 5902MB.<br /><br />También puede usar la memoria dinámica. Para obtener más información, consulte [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).|  
|**Configurar redes**|No conectado|Puede seleccionar una conexión de red para la máquina virtual para usarla en una lista de conmutadores virtuales existentes. Consulte [crear un conmutador virtual para las máquinas virtuales de Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).|  
|**Conectar disco duro virtual**|Crear un disco duro virtual<br /><br />Nombre: <*vmname*> .vhdx<br /><br />**Ubicación**: **Discos duros C:\Users\Public\Documents\Hyper-V\Virtual\\**<br /><br />**Tamaño**: 127GB|También puede usar un disco duro virtual existente o esperar y adjuntar un disco duro virtual más tarde.|  
|**Opciones de instalación**|Instalar un sistema operativo más adelante|Estas opciones cambian el orden de arranque de la máquina virtual para que pueda instalar desde un archivo .iso, disquete de arranque o un servicio de instalación de red, como los servicios de implementación de Windows (WDS).|  
|**Resumen**|Muestra las opciones que ha elegido, para que pueda comprobar que son correctos.<br /><br />-Nombre<br />-Generación<br />-Memoria<br />: Red<br />-Disco<br />: El sistema de operativo|**Sugerencia:** Puede copiar el resumen de la página y pegarla en el correo electrónico o en otro lugar que le ayudarán a realizar un seguimiento de las máquinas virtuales.|  

## <a name="see-also"></a>Vea también  

- [New-VM](https://technet.microsoft.com/library/hh848537.aspx)  

- [Versiones de configuración de máquina virtual admitidas](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#BKMK_SupportedConfigVersions)  

-   [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [Crear un conmutador virtual para las máquinas virtuales de Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  
