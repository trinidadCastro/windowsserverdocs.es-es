---
title: Administrar BranchCache en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 13d1d439eb9eeb60de9779d783e36405aee3ddfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Administrar BranchCache en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

BranchCache puede ayudarte a optimizar el uso de Internet, mejorar el rendimiento de aplicaciones en red y reduce el tráfico en la red de área extensa (WAN) cuando el servidor de Windows Server Essentials se encuentra remotamente desde la oficina o cuando los equipos cliente conecten a un servidor local usar recursos basados en la nube, como las bibliotecas de SharePoint Online.  
  
 Con BranchCache habilitado, cuando un equipo cliente solicita contenido desde un servidor remoto de Windows Server Essentials, el contenido se almacena en caché en la oficina local. Después, otros equipos en la misma oficina pueden obtener el contenido local en lugar de descargar el contenido desde el servidor vuelva a través de la WAN. Esto puede mejorar el rendimiento de aplicaciones en red y reduce el consumo de ancho de banda a través de la WAN.  
  
 Si el servidor de Windows Server Essentials es local o remota, BranchCache puede mejorar los tiempos de respuesta para carpetas del servidor compartido y contenido Web hospedado en el servidor (como las bibliotecas de SharePoint Online).  
  
 Ya no requiere BranchCache nuevo hardware o cambios de la topología de red, esta característica te ofrece una forma sencilla de optimizar el uso de ancho de banda y mejorar los tiempos de respuesta para servicios y recursos que se accede a través de la WAN.  
  
## <a name="branchcache-scenarios"></a>BranchCache escenarios  
 Hay tres escenarios básicos para usar BranchCache con un servidor remoto:  
  
-   El servidor de Windows Server Essentials se hospeda en Microsoft Azure.  
  
-   El servidor de Windows Server Essentials está hospedado en el centro de datos de un proveedor de servicios de terceros.  
  
-   El servidor de Windows Server Essentials es en otra oficina en una ubicación física diferente.  
  
## <a name="distributed-cache-mode"></a>Modo de caché distribuida  
 En Windows Server Essentials, BranchCache se implementa en *modo de caché distribuida*, uno de dos modos de caché disponibles en BranchCache. En el modo de caché distribuida, la memoria caché de contenido en las sucursales se distribuye entre los equipos cliente. Puesto que no están necesarios ningún hardware adicional o cambios en la topología, este modo funciona bien para oficinas pequeñas que usan un servidor remoto o usan un servidor local para acceder a servicios basados en la nube como SharePoint Online. Cuando activas BranchCache en Windows Server Essentials, se implementa el modo de caché distribuida.  
  
> [!NOTE]
>  En las sucursales más grandes con más de una subred o con un gran número de los empleados que usen aplicaciones de red, puede ser útil implementar BranchCache en *hospedado en modo de caché*. En el modo de la memoria caché hospedada, la memoria caché de contenido se almacena en uno o varios servidores de la memoria caché hospedada en las sucursales.
  
## <a name="requirements"></a>Requisitos  
 Para usar BranchCache en Windows Server Essentials, los equipos cliente y servidor deben cumplir los siguientes requisitos:  
  
-   El servidor debe ejecutar el sistema operativo de Windows Server Essentials o el Windows Server 2012 R2 Standard o sistema operativo de Windows Server 2012 R2 Datacenter con el rol de Windows Server Essentials Experience.  
  
     En un servidor de Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, BranchCache se agrega al agregar el rol de Windows Server Essentials Experience. Para activar BranchCache, tendrás que iniciar sesión en el escritorio de Windows Server Essentials con credenciales de administrador de dominio.  
  
-   Los equipos cliente deben ejecutar la Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Enterprise o sistema operativo Windows 8.1 Enterprise.  
  
-   En el modo de caché distribuida, deben ser todos los equipos cliente en la misma subred.  
  
    > [!NOTE]
    >  Si los equipos cliente había conectado al servidor de Windows Server Essentials sin ellos unirse al dominio, los equipos están excluidos de almacenamiento en caché de forma predeterminada. Para incluir un equipo cliente que no está unido a dominio en el almacenamiento en caché, ejecutar la [habilitar BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) cmdlet de Windows PowerShell en el equipo cliente. Para obtener más información, consulta [BranchCache Cmdlets de Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).  
 
  
## <a name="turn-branchcache-on"></a>Activar BranchCache  
 Para activar BranchCache en modo de caché distribuido, simplemente haz clic en un botón en el panel de Windows Server Essentials. Almacenamiento en caché comienza de inmediato y se realiza de forma transparente.  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Para activar BranchCache en Windows Server Essentials  
  
1.  Inicia sesión en el servidor de Windows Server Essentials con tu cuenta de administrador.  
  
2.  En el panel de Windows Server Essentials, haz clic en **configuración**.  
  
     Abre el Asistente de configuración.  
  
3.  Haz clic en **BranchCache**.  
  
4.  En la **BranchCache configuración** página, haz clic en **activar**.  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Usar Windows PowerShell para activar o desactivar la BranchCache  
 Puedes usar Windows PowerShell para comprobar el estado de BranchCache (activado o desactivado) y para activar o desactivar la BranchCache.  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Para activar BranchCache o desactivar mediante Windows PowerShell  
  
1.  En el servidor, abre Windows PowerShell como administrador. En la **inicio** página, haz clic en **Windows PowerShell**, haz clic en **ejecutar como administrador**y, a continuación, haz clic en **Sí**.  
  
2.  Escribir cualquiera de los siguientes cmdlets en el símbolo del sistema.  
  
    -   Para comprobar el estado de BranchCache (activado o desactivado), escribe:  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   Para activar BranchCache, escribe:  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   Para desactivar BranchCache, escribe:  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>Consulta también  
    
-   [Introducción a BranchCache](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
