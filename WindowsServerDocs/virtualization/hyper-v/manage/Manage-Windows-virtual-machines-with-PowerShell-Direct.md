---
title: Administrar las máquinas virtuales de Windows con PowerShell Direct
description: Proporciona instrucciones para usar PowerShell Direct para administrar las máquinas virtuales sin tener que depender de una red o una conexión remota con ellos.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc09093ba2d
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 4081a9737825d2f50f0d3b19b3bada3b9bbc76f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814706"
---
# <a name="manage-windows-virtual-machines-with-powershell-direct"></a>Administrar las máquinas virtuales de Windows con PowerShell Direct

>Se aplica a: Windows 10, Windows Server 2016, Windows Server de 2019
  
Puede usar PowerShell Direct para administrar de forma remota un Windows 10, Windows Server 2016 o Windows Server 2019 máquina virtual desde un host de Windows 10, Windows Server 2016 o Windows 2019 Hyper-V Server. PowerShell Direct permite la administración de Windows PowerShell dentro de una máquina virtual, independientemente de la configuración de red o la configuración de administración remota en el host de Hyper-V o la máquina virtual. Esto facilita a los administradores de Hyper-V la automatización y creación de scripts de configuración y administración de las máquinas virtuales.  
  
Hay dos maneras de ejecutar PowerShell Direct:  
  
- Crear y salir de una sesión de PowerShell Direct mediante los cmdlets de PSSession
  
- Ejecute el script o un comando con el cmdlet Invoke-Command
  
Si está administrando máquinas virtuales antiguas, use la opción Conexión a máquina virtual (VMConnect) o [configure una red virtual para la máquina virtual](https://technet.microsoft.com/library/cc816585.aspx).  
  
## <a name="create-and-exit-a-powershell-direct-session-using-pssession-cmdlets"></a>Crear y salir de una sesión de PowerShell Direct mediante los cmdlets de PSSession  
  
1. En el host de Hyper-V, abra Windows PowerShell como administrador.  
  
2. Use la [Enter-PSSession](https://technet.microsoft.com/library/hh849707.aspx) para conectarse a la máquina virtual. Ejecute uno de los siguientes comandos para crear una sesión con el nombre de la máquina virtual o el GUID:  
  
    ```  
    Enter-PSSession -VMName <VMName>  
    ```  
  
    ```  
    Enter-PSSession -VMGUID <VMGUID>  
    ```  
  
3. Escriba sus credenciales para la máquina virtual.   
4. Ejecute los comandos que necesite. Estos comandos se ejecutan en la máquina virtual con la que creó la sesión.  
  
5.  Cuando haya terminado, use el [Exit-PSSession](https://technet.microsoft.com/library/hh849743.aspx) para cerrar la sesión.   
  
    ```  
    Exit-PSSession  
    ```  
  
## <a name="run-script-or-command-with-invoke-command-cmdlet"></a>Ejecute el script o un comando con el cmdlet Invoke-Command  
Puede usar el cmdlet [Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command) para ejecutar un conjunto predeterminado de comandos en la máquina virtual. Este es un ejemplo de cómo puede usar el cmdlet Invoke-Command, donde PSTest es el nombre de la máquina virtual y el script que se ejecutará (foo.ps1) está en la carpeta script de la unidad C:/:  
  
```  
Invoke-Command -VMName PSTest  -FilePath C:\script\foo.ps1  
```  
  
Para ejecutar un único comando, use el parámetro **-ScriptBlock**:  
  
```  
Invoke-Command -VMName PSTest  -ScriptBlock { cmdlet }  
```  
  
## <a name="whats-required-to-use-powershell-direct"></a>¿Lo que se requiere para usar PowerShell Direct?  
Para crear una sesión de PowerShell Direct en una máquina virtual,  
  
-   La máquina virtual debe ejecutarse localmente en el host y estar iniciada.  
  
-   Debe iniciar sesión en el equipo host como un administrador de Hyper-V.  
  
-   Debe facilitar credenciales de usuario válidas para la máquina virtual.  
  
-   El sistema operativo host debe ejecutar al menos Windows 10 o Windows Server 2016.
  
-   La máquina virtual debe ejecutar al menos Windows 10 o Windows Server 2016.  
  
Puede usar el [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet para comprobar que las credenciales que usa tengan el rol de administrador de Hyper-V y para obtener una lista de las máquinas virtuales que se ejecutan localmente en el host y están iniciadas.  
  
## <a name="see-also"></a>Vea también  
[Enter-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enter-PSSession)  
[Exit-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Exit-PSSession)  
[Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command)  
  


