---
title: Administración de máquinas virtuales Windows con PowerShell Direct
description: Proporciona instrucciones sobre el uso de PowerShell Direct para administrar máquinas virtuales sin depender de una red o conexión remota a ellas.
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc09093ba2d
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: c6879f6c7762b7a4e811db8aaa001444be0b6688
ms.sourcegitcommit: 5f234fb15c1d0365b60e83a50bf953e317d6239c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879864"
---
# <a name="manage-windows-virtual-machines-with-powershell-direct"></a>Administración de máquinas virtuales Windows con PowerShell Direct

>Se aplica a: Windows 10, Windows Server 2016, Windows Server 2019

Puede usar PowerShell Direct para administrar de forma remota una máquina virtual de Windows 10, Windows Server 2016 o Windows Server 2019 desde un host de Hyper-V de Windows 10, Windows Server 2016 o Windows Server 2019. PowerShell Direct permite la administración de Windows PowerShell dentro de una máquina virtual, independientemente de la configuración de la red o la configuración de administración remota en el host de Hyper-V o la máquina virtual. Esto facilita a los administradores de Hyper-V la automatización y creación de scripts de configuración y administración de las máquinas virtuales.

Hay dos maneras de ejecutar PowerShell Direct:

- Crear y salir de una sesión de PowerShell Direct mediante cmdlets de PSSession

- Ejecutar script o comando con el cmdlet Invoke-Command

Si está administrando máquinas virtuales antiguas, use la opción Conexión a máquina virtual (VMConnect) o [configure una red virtual para la máquina virtual](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816585(v=ws.10)).

## <a name="create-and-exit-a-powershell-direct-session-using-pssession-cmdlets"></a>Crear y salir de una sesión de PowerShell Direct mediante cmdlets de PSSession

1. En el host de Hyper-V, abra Windows PowerShell como administrador.

2. Use el cmdlet [Enter-PSSession](/powershell/module/microsoft.powershell.core/enter-pssession?view=powershell-7&preserve-view=true) para conectarse a la máquina virtual. Ejecute uno de los siguientes comandos para crear una sesión mediante el GUID o el nombre de la máquina virtual:

    ```
    Enter-PSSession -VMName <VMName>
    ```

    ```
    Enter-PSSession -VMGUID <VMGUID>
    ```

3. Escriba las credenciales de la máquina virtual.
4. Ejecute los comandos que necesite. Estos comandos se ejecutan en la máquina virtual con la que creó la sesión.

5.  Cuando haya terminado, use [Exit-PSSession](/powershell/module/microsoft.powershell.core/exit-pssession?view=powershell-7&preserve-view=true) para cerrar la sesión.

    ```
    Exit-PSSession
    ```

## <a name="run-script-or-command-with-invoke-command-cmdlet"></a>Ejecutar script o comando con Invoke-Command cmdlet
Puede usar el cmdlet [Invoke-Command](/powershell/module/Microsoft.PowerShell.Core/Invoke-Command) para ejecutar un conjunto predeterminado de comandos en la máquina virtual. Este es un ejemplo de cómo puede usar el cmdlet Invoke-Command, donde PSTest es el nombre de la máquina virtual y el script que se ejecutará (foo.ps1) está en la carpeta script de la unidad C:/:

```
Invoke-Command -VMName PSTest  -FilePath C:\script\foo.ps1
```

Para ejecutar un único comando, use el parámetro **-ScriptBlock**:

```
Invoke-Command -VMName PSTest  -ScriptBlock { cmdlet }
```

## <a name="whats-required-to-use-powershell-direct"></a>¿Qué se requiere para usar PowerShell Direct?
Para crear una sesión de PowerShell Direct en una máquina virtual,

-   La máquina virtual debe ejecutarse localmente en el host y estar iniciada.

-   Debe iniciar sesión en el equipo host como un administrador de Hyper-V.

-   Debe facilitar credenciales de usuario válidas para la máquina virtual.

-   El sistema operativo host debe ejecutar al menos Windows 10 o Windows Server 2016.

-   La máquina virtual debe ejecutar al menos Windows 10 o Windows Server 2016.

Puede usar el cmdlet [Get-VM](/powershell/module/hyper-v/get-vm) para comprobar que las credenciales que está usando tienen el rol de administrador de Hyper-V y para obtener una lista de las máquinas virtuales que se ejecutan localmente en el host y se inician.

## <a name="see-also"></a>Consulte también
[Enter-PSSession](/powershell/module/Microsoft.PowerShell.Core/Enter-PSSession) 
 [Exit-PSSession](/powershell/module/Microsoft.PowerShell.Core/Exit-PSSession) 
 [Invoke-Command](/powershell/module/Microsoft.PowerShell.Core/Invoke-Command)
