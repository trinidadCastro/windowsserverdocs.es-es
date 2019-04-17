---
title: Características eliminadas o en desuso en Windows Server 2016
description: Características y funcionalidades se han quitado o planeado para su eliminación en las versiones.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/02/2017
ms.assetid: 5d10c5f9-ebac-49a0-b808-c0b1702e0437
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 20178a3be14c076623f647fa139e013528de9a69
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133891"
---
# Características eliminadas o en desuso en Windows Server 2016

>Se aplica a: Windows Server 2016

A continuación se enumeran las características y funcionalidades de Windows Server 2016 que se quitaron del producto en la versión actual o cuya eliminación se planea para las siguientes versiones (en desuso). Este artículo está orientado a profesionales de TI que actualizan sus sistemas operativos en un entorno comercial. Esta lista está sujeta a cambios en versiones posteriores y es posible que no incluya todas las características o funcionalidades desusadas. Para obtener más detalles sobre una característica o funcionalidad en concreto y su reemplazo, consulte la documentación relativa a la característica en cuestión.  

## Características quitadas de Windows Server 2016 
Las siguientes características y funcionalidades se quitaron de esta versión de Windows Server 2016. Las aplicaciones, el código o el uso que dependen de estas características no funcionarán en esta versión, a menos que se emplee un método alternativo.  

> [!NOTE]  
> Si va a pasar a Windows Server 2016 desde una versión de servidor anterior a Windows Server 2012 R2 o Windows Server 2012, también debe consultar [Características quitadas o desusadas en Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx) y [Características o funcionalidades desusadas en Windows Server 2012](https://technet.microsoft.com/library/hh831568.aspx).  


### Servidor de archivos  
El complemento de administración de almacenamiento y recursos compartidos para Microsoft Management Console se ha quitado. En su lugar, lleve a cabo cualquiera de las siguientes acciones:  

-   Si el equipo que desea administrar está ejecutando un sistema operativo anterior a Windows Server 2016, conéctese a él con Escritorio remoto y use la versión local del complemento de administración de almacenamiento y recursos compartidos.  

-   En un equipo que ejecuta Windows 8.1 o versiones anteriores, utilice el complemento de administración de almacenamiento y recursos compartidos desde RSAT para ver el equipo que quiere administrar.  

-   Utilice Hyper-V en un equipo cliente para ejecutar una máquina virtual con Windows 7, Windows 8 o Windows 8.1 con el complemento de administración de almacenamiento y recursos compartidos de RSAT.  

### Journal.dll  
El archivo Journal.dll se ha suprimido en Windows Server 2016. No hay sustitución.  

### Asistente para configuración de seguridad  
El Asistente para configuración de seguridad se ha quitado. En su lugar, se protegen las características de forma predeterminada. Si necesita controlar la configuración de seguridad específica, puede usar una directiva de grupo o el [Administrador de cumplimiento de normas de seguridad de Microsoft](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx).  

### SQM  
Se han quitado los componentes de participación que administran la participación en el Programa para la mejora de la experiencia del usuario. 

### Windows Update
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

## Características en desuso a partir de Windows Server 2016 
Las siguientes características y funcionalidades quedarán en desuso a partir de esta versión. Con el tiempo, se quitarán completamente del producto, pero aún están disponibles en esta versión (en algunos casos, se les ha quitado cierta funcionalidad). Debe comenzar a planear el empleo de métodos alternativos para cualquier aplicación, código o uso que dependa de estas características.  

### Herramientas de configuración  

-   **Scregedit.exe** está en desuso. Si tiene scripts que dependen de Scregedit.exe, ajústelos para que utilicen los métodos Reg.exe o de Windows PowerShell.  

-   **Sconfig.exe** está en desuso. Utilice Windows PowerShell en su lugar.  

### API personalizadas de NetCfg  
La instalación de PrintProvider, NetClient e ISDN mediante las API personalizadas de NetCfg está en desuso.  

### Administración remota  
WinRM.vbs está en desuso. En su lugar, utilice la funcionalidad del proveedor de WinRM de Windows PowerShell.  

### SMB  
SMB 2+ en NetBT está en desuso. En su lugar, implementa SMB sobre TCP o RDMA. 
