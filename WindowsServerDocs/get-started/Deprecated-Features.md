---
title: Características eliminadas o en desuso en Windows Server 2016
description: Una lista de características y funcionalidades de Windows Server 2016 que se quitaron del producto en la versión actual o se planifican su posible eliminación en las versiones posteriores (en desuso). Este artículo está orientado a profesionales de TI que actualizan sus sistemas operativos en un entorno comercial.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.date: 05/21/2019
ms.assetid: 5d10c5f9-ebac-49a0-b808-c0b1702e0437
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 83855cf7e4fa86a932298dd15735dc5bf7277dfb
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976605"
---
# <a name="features-removed-or-deprecated-in--windows-server-2016"></a>Características eliminadas o en desuso en Windows Server 2016

>Se aplica a: Windows Server 2016

A continuación se enumeran las características y funcionalidades de Windows Server 2016 que se quitaron del producto en la versión actual o cuya eliminación se planea para las siguientes versiones (en desuso). Este artículo está orientado a profesionales de TI que actualizan sus sistemas operativos en un entorno comercial. Esta lista está sujeta a cambios en versiones posteriores y es posible que no incluya todas las características o funcionalidades desusadas. Para obtener más detalles sobre una característica o funcionalidad en concreto y su reemplazo, consulte la documentación relativa a la característica en cuestión.

Para obtener información sobre lo que se han quitado o en desuso en las versiones más recientes, consulte [características eliminadas o planeado para el reemplazo a partir de Windows Server 2019](../get-started-19/removed-features-19.md).

## <a name="features-removed-from-windows-server-2016"></a>Características quitadas de Windows Server 2016

Las siguientes características y funcionalidades se quitaron de esta versión de Windows Server 2016. Las aplicaciones, el código o el uso que dependen de estas características no funcionarán en esta versión, a menos que se emplee un método alternativo.  

> [!NOTE]  
> Si va a pasar a Windows Server 2016 desde una versión de servidor anterior a Windows Server 2012 R2 o Windows Server 2012, también debe consultar [Características quitadas o desusadas en Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx) y [Características o funcionalidades desusadas en Windows Server 2012](https://technet.microsoft.com/library/hh831568.aspx).  


### <a name="file-server"></a>Servidor de archivos  
El complemento de administración de almacenamiento y recursos compartidos para Microsoft Management Console se ha quitado. En su lugar, lleve a cabo cualquiera de las siguientes acciones:  

-   Si el equipo que desea administrar está ejecutando un sistema operativo anterior a Windows Server 2016, conéctese a él con Escritorio remoto y use la versión local del complemento de administración de almacenamiento y recursos compartidos.  

-   En un equipo que ejecuta Windows 8.1 o versiones anteriores, utilice el complemento de administración de almacenamiento y recursos compartidos desde RSAT para ver el equipo que quiere administrar.  

-   Utilice Hyper-V en un equipo cliente para ejecutar una máquina virtual con Windows 7, Windows 8 o Windows 8.1 con el complemento de administración de almacenamiento y recursos compartidos de RSAT.  

### <a name="journaldll"></a>Journal.dll  
El archivo Journal.dll se ha suprimido en Windows Server 2016. No hay sustitución.  

### <a name="security-configuration-wizard"></a>Asistente para configuración de seguridad  
El Asistente para configuración de seguridad se ha quitado. En su lugar, se protegen las características de forma predeterminada. Si necesitas controlar la configuración de seguridad específica, puedes usar una Directiva de grupo o el [Administrador de cumplimiento de normas de seguridad de Microsoft](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx).  

### <a name="sqm"></a>SQM  
Se han quitado los componentes de participación que administran la participación en el Programa para la mejora de la experiencia del usuario. 

### <a name="windows-update"></a>Windows Update
Se ha quitado el comando **wuauclt.exe /detectnow** y ya no se admite. Para desencadenar una búsqueda de actualizaciones, realiza una de las siguientes acciones:

- Ejecuta estos comandos de PowerShell:
    ````powershell
    $AutoUpdates = New-Object -ComObject "Microsoft.Update.AutoUpdate"`
    $AutoUpdates.DetectNow()` 
    ````

- Como alternativa, usa este VBScript:
    ````vb
    Set automaticUpdates = CreateObject("Microsoft.Update.AutoUpdate")
    automaticUpdates.DetectNow()
    ````

## <a name="features-deprecated-starting-with-windows-server-2016"></a>Características desusadas a partir de Windows Server 2016 
Las siguientes características y funcionalidades quedarán en desuso a partir de esta versión. Con el tiempo, se quitarán completamente del producto, pero aún están disponibles en esta versión (en algunos casos, se les ha quitado cierta funcionalidad). Debe comenzar a planear el empleo de métodos alternativos para cualquier aplicación, código o uso que dependa de estas características.  

### <a name="configuration-tools"></a>Herramientas de configuración  

-   **Scregedit.exe** está en desuso. Si tiene scripts que dependen de Scregedit.exe, ajústelos para que utilicen los métodos Reg.exe o de Windows PowerShell.  

-   **Sconfig.exe** está en desuso. Utilice Windows PowerShell en su lugar.  

### <a name="netcfg-custom-apis"></a>API personalizadas de NetCfg  
La instalación de PrintProvider, NetClient e ISDN mediante las API personalizadas de NetCfg está en desuso.  

### <a name="remote-management"></a>Administración remota  
WinRM.vbs está en desuso. En su lugar, utilice la funcionalidad del proveedor de WinRM de Windows PowerShell.  

### <a name="smb"></a>SMB  
SMB 2+ en NetBT está en desuso. En su lugar, implemente SMB sobre TCP o RDMA. 
